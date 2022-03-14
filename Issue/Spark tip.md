#### Spring batch 프로그램에서 spark를 bean 선언 없이 매 배치마다 생성하기
- 상황 설명 :
jobParameters를 생성하여 batch 프로그램을 실행하고 있다.    
각 job 들은 spark를 사용하고 있고 매번 spark 를 생성하기 보다 bean 으로 sparkConf, JavaSparkContext 한번 선언하고 계속 사용하고 있다.    
하지만 spark에서 에러가 발생했을 때 프로그램을 종료하고 다시 실행해야만 하는 문제가 발생하게 되었고 이에 대한 방안을 제안하는 바이다.

```java
@Configuration
public class SparkConfig {
    @Bean
    public JavaSparkContext sparkContext() {
        SparkConf sparkConf = new SparkConf().setMaster("local[*]")
                .set("spark.local.dir", "/tmp/spark-temp")
                .set("spark.executor.memory", "12g")
                .set("spark.network.timeout", "600000ms")
//                .set("spark.driver.memory", "6g")
//                .set("spark.memory.offHeap.enabled", "true")
//                .set("spark.memory.offHeap.size", "12g")
                .set("spark.driver.maxResultSize", "0")
//                .set("spark.worker.cleanup.enabled", "true")
//                .set("spark.worker.cleanup.interval", "11s")
//                .set("spark.shuffle.service.db.enabled", "false")
                .setAppName("DetectSparkApp");

        return new JavaSparkContext(sparkConf);
    }
}
```
기존에 JavaSparkContext를 사용하는 방법
```java
@Autowired
    private JavaSparkContext sparkContext;

```
- 기존 방법의 장점 : 
자주 사용하는 spark를 한번 bean으로 사용함으로써 spark를 생성하고 삭제하는 등의 리소스를 줄일 수 있음.    
spark는 standalone , Serializable한 서비스를 선호하기 때문에 bean으로 선언해주면 1개의 sparkConfig임을 명시할 수 있음.    
    
- 단점, 문제 인식 : 

spark는 서비스 중 예상치 못한 에러가 발생되었을 때 spark-temp 디렉터리에 문제가 생김.     
하지만 프로그램이 종료되지 않고 다음 batch job 에서 동일한 spark-temp location을 호출함으로써 fileNotFoundException 에러가 뜸.    
→ 실제로 Spark 처리 과정 중 GSON에러가 나게 되면 sark-temp 가 삭제되기도 하고 아니기도 함...    
→ 보통 이런 경우, spark-temp 폴더를 삭제하고 프로그램을 재시작하는 것이 메뉴얼    
이를 해결하기 위해서는 에러가 발생할 때마다 spark-temp 폴더를 삭제하고, job이 생성될 때마다 spark를 생성해야 함.    
이때, spark 가 serializable 할 수 있도록 코드를 작성하는 것도 잊지 않아야 한다.   
    
참고 : https://java8.tistory.com/39    
```java
@Configuration
public class SparkConfig {
   @Autowired
   private ServiceInfo serviceInfoConfig;
   private static JavaSparkContext javaSparkContext;

   @Bean
   public void SparkConfig() {
       setSparkContext();
   }

   public void setSparkContext() {
       SparkConf sparkConf = new SparkConf().setMaster("local[*]")
               .set("spark.local.dir", "/tmp/spark-temp")
               .set("spark.executor.memory", "12g")
               .set("spark.executor.cores", serviceInfoConfig.getSparkCores())
               .set("spark.network.timeout", "600000ms")
               .set("spark.driver.maxResultSize", "0")
               .set("spark.worker.cleanup.enabled", "true")
               .set("spark.worker.cleanup.interval", "10000ms")
               .set("spark.shuffle.service.db.enabled", "true")
               .setAppName("DetectSparkApp");

       javaSparkContext = new JavaSparkContext(sparkConf);
   }

   public JavaSparkContext getJavaSparkContext() {
       return javaSparkContext;
   }

   public void resetSparkContext() {
       javaSparkContext.close();
       setSparkContext();
   }
}
```
```java
private transient JavaSparkContext sparkContext;
public void setSparkContext(JavaSparkContext sparkContext) {
        this.sparkContext = sparkContext;
    }

```
- tips :
Bean  상태로 job parameter를 전달할 수 없다.  
→ Bean 어노테이션을 다 지우던가, Bean 이 아닌 메소드로 javaSparkContext를 세팅한다.
→ transient를 추가함으로써 serializable 하게 sparkConfig를 전달한다.

<br/><br/><br/><br/>

##### Spark Executor 에서 Exception 잡기


- 상황 설명 :    
mapToPair 메소드 안에서 error exception이 발생했을 때 exception을 제대로 catch 하지 않는다.    

```java
public class DetectItemProcessor implements ItemProcessor<JoinInfo, List<FileInfo>> {
	...
    private transient JavaSparkContext sparkContext;
    public DetectItemProcessor(JavaSparkContext sparkContext) {
        this.sparkContext = sparkContext;
    }

    @Override
    public List<FileInfo> process(final JoinInfo info) {
	...
      try {
			...
            JavaRDD<String> newLines = sparkContext.textFile(currentResultPath);
            JavaPairRDD<Object, FileInfo> pairNewLines = newLines.mapToPair(line -> {
                    FileInfo fileInfo = gson.fromJson(line, FileInfo.class);
                    return new Tuple2<>(fileInfo.getKey(), fileInfo);
			});
			...
 		} catch (Exception e) {
            e.printStackTrace();
        }
        return list;
    }
}

```


mapToPair 안에서 Exception이 3번 발생되어도 catch에서 1개만 catch 되며 에러문은 sparkException이다.    
GSON 으로 생긴 에러 (com.google.gson.JsonSyntaxException) 라고 해도 sparkException만 throw 된다.    
하지만 catch로 잡을 수 없다.. catch(SparkException) 안됨.


- 목표 : 모든 Exception list를 받기
모든 Exception을 잡을 필요가 있을 때를 대비하여 한번 방안을 찾아보았다.    
spark 의 mapToPair와 같은 run 메소드는 위 로그를 보면 알다시피 spark의 executor에서 작동된다.    
spark의 mapToPair, map, groupBykey와 같이 translated RDD를 하는 메소드는 바로 실행되지 않고 spark에서 Action을 하는 메소드인 print, count collect 등이 호출되면 그때 동작한다.    
그 이유는 spark 내부에서 RDD를 translate 할 때 action에 최적화하게 위해서이다.    
  - spark 가 아닌 직접 병렬 Thread 를 선언했을 때, 병렬 Thread의 결과값을 취합해야 하는 상황일 때 Future를 사용해서 처리한다.    
  - spark 의 경우 결과값을 serialize 하게 받아오기 위해서는 RDD 를 한번 collect() 해야 한다.


<br/><br/>

코드 상에서는 mapToPair 가 try 안에 있지만,     
병렬 쓰레드에서 작업되기에 에러가 throw 될 때 catch로 잡지 않고 병렬 쓰레드 안에서 throw catch 하고(getter) 최종 sparkException으로 throw 한다.    

<br/>
(참고) spark 의 translated RDD 작업들은 한 RDD가 실패했을 때 이전 RDD로 retry를 보장해준다.    
main try-catch에서는 내부에 exception 이 하나만 발견되어도 잡기 때문에 결론적으로 여러 exception 이 발생되더라도 하나만 잡는다.    
당연하게도 한 spark 병렬 쓰레드에서 exception이 Throw 된다고 해서 mapToPiar 작업을 종료시킬 수 없다.    
이는 spark 가 아니라 직접 병렬 쓰레드를 만든다고 해도 같은 현상이 일어난다.    

참조 : https://anish749.github.io/spark/exception-handling-spark-data-frames/




- 삽질 1

병렬 쓰레드에서 Exception을 상위 level (정확히는 상위 level이라고 하면 안된다고 함) 에서 catch 하는 방법은 stackoverflow에서 참고함 (https://stackoverflow.com/questions/6546193/how-to-catch-an-exception-from-a-thread)    

방식은 exception이 발생될 때 해당 exception 클래스를 atomic 한 전역변수 리스트에 저장함.    

```java
public class DetectItemProcessor implements ItemProcessor<JoinInfo, List<FileInfo>> {
    private static Gson gson = new Gson();
    private transient JavaSparkContext sparkContext;
//  셋 중 하나 선택...
//    private static Map<Thread, Queue<Exception>> exceptions =  Collections.synchronizedMap(new HashMap<Thread, Queue<Exception>>());
//    private static List<Exception> exceptions = Collections.synchronizedList(new ArrayList<>());
    private static Queue<Exception> exceptions = new ConcurrentLinkedQueue<>();

    public DetectItemProcessor(JavaSparkContext sparkContext) {
        this.sparkContext = sparkContext;
    }

    @Override
    public List<FileInfo> process(final JoinInfo info) {
	...
      try {
			...
            JavaRDD<String> newLines = sparkContext.textFile(currentResultPath);
            JavaPairRDD<Object, FileInfo> pairNewLines = newLines.mapToPair(line -> {
                try {
                    FileInfo fileInfo = gson.fromJson(line, FileInfo.class);
                    return new Tuple2<>(fileInfo.getKey(), fileInfo);
                } catch (Exception e) {
                    log.error("catch Exception while reading JSON: {}", line);
                    log.error(e.getMessage(), e);
					exception.add(e);
                    return null;
                }
            });
			...
			pairnewLines.collect();
   			if(exceptions.peek()!=null){
                 throw excpetions.poll();
            }
			...
 		} catch (Exception e) {
            e.printStackTrace();
        }
        return list;
    }
}
```

exception을 저장하는 전역 변수는 위와 같이 atmoic 할 수 있도록 한다.    
exceptions 들을 전역 변수로 하는 이유는 mapToPair 는 로컬 변수를 참조할 수 없기 때문이다.    
mapToPair를 살펴보면 아래와 같고 원래는 lamda 형식이 아닌 PairFuncion을 직접 new 생성해주어야 했다.

```java
// mapToPair class
@FunctionalInterface
public interface PairFunction<T, K, V> extends Serializable {
    Tuple2<K, V> call(T var1) throws Exception;
}
```
```java
// pariFunction 예시
 JavaPairRDD<Object, FileInfo> pairOldLines = oldLines.mapToPair(new PairFunction<String, Object, FileInfo>() {
                                @Override
                                public Tuple2<Object, FileInfo> call(String s) throws Exception {
                                    FileInfo fileInfo = gson.fromJson(line, FileInfo.class);
                                    return new Tuple2<>(fileInfo.getKey(), fileInfo);
                                }
                            });
```
원래 위와 같이 PairFunction은 매개변수를 받지 않는 형식이기 때문에 로컬 변수를 참조할 수 없다.    
잘 작동이 되지만 원칙적으로 Spark에서는 지역변수든 전역변수든 노드들이 외부 변수를 참조하는 것을 권장하지 않는다.    
<br/>
안되는 이유를 atomic을 빼보면 느낄 수 있는데,    
atomic을 빼고 동작하게 될 때 execution내에서는 리스트를 공통적으로 잘 적재하는 것처럼 보이나,,     
최종 collect, count 등을 하게 되면 list value 가 0이 된다.


<br/><br/>

- 삽질 2 
spark 공식 document에서는 spark에서 executor들의 자원을 참조하기 위해서는 accumulator와 broadcast를 사용하는 것이 안전한 api라고 안내해주고 있다.    
그래서 broadcast 클래스가 참조되어 있는 sparkListener를  extends로 시도해 보았다.    
잘 작동은 하지만.. 안타깝게도 broadcast, accumulator 메소드들은 부하도 생길 뿐더러 java에서 권장하지 않는다고 한다.    
실제로 accumulator 메소드는 deprecated 되어있다.    
<br/>
다른 방법이 있을 수 있는데, RDD 결과값을 collect를 이용해서 Exception을 찾아보는 것이다.

```java
public class DetectItemProcessor implements ItemProcessor<JoinInfo, List<FileInfo>> {
    private static Gson gson = new Gson();
    private transient JavaSparkContext sparkContext;

    public DetectItemProcessor(JavaSparkContext sparkContext) {
        this.sparkContext = sparkContext;
    }

    @Override
    public List<FileInfo> process(final JoinInfo info) {
	...
      try {
			...             
			JavaRDD<String> oldLines = sparkContext.textFile(priorResultPath);
            JavaPairRDD<Object, FileInfo> pairOldLines = oldLines.mapToPair(line -> {
                   try {
                      FileInfo fileInfo = gson.fromJson(line, FileInfo.class);
                      return new Tuple2<>(fileInfo.getKey(), fileInfo);
                   } catch (Exception e) {
                      log.error("catch Exception while reading JSON : {}", line);
                      log.error(e.getMessage(), e);
                      return new Tuple2<>(e,null);
                   }
            });

            for(Iterator<Tuple2<Object, FileInfo>> iterator = pairOldLines.collect().listIterator(); iterator.hasNext();){
                  if(iterator.next()._1.getClass()!=String.class){
                 	 throw (Exception) iterator.next()._1();
                  }
             }    
			...
 		} catch (SparkException e) {
            log.error(e.getMessage(), e);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return list;
    }
}
```

- 그 외 시도들..

추가적으로 countdownlatch 도 시도해보았다.    
참고 : https://www.linkedin.com/pulse/spark-launcher-amol-kale.     

sparklauncher를 만들고, SparkAppListener를 직접 생성하는데, 이때 countdownlatch를 참조하는 식이었다.    
countdownlatch의 기능은 쓰레드의 상태를 보고 await 하거나 다음 작업으로 넘겨주는 등이다.    

쓰레드를 await 한다고 했을 때, mapToPair가 실패했을 때 countdown을 해야할지, 성공할 때 countdown을 해야할지 모호하며     
await 말고는 특별한 기능이 없기 때문에 적용하지 않았다.




+ mapToPair와 .collect().forEach의 exception 결과가 다른 이유.   
.collect().forEach(data→{ .... }) 에서 exception이 발생했을 때는 곧바로 main catch에서 잡히며 모든 쓰레드가 종료된다.    

그 이유는 collect() 자체가 list 를 반환하며 이는 단일 쓰레드이기 때문에 가능하다.    

executor 내부에서 stdout 은 사용되지 않으며 안전하게 출력되기 위해서는 .foreach를 사용할 것을 권장한다고 한다...




