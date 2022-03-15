## tasklet에서 @service를 호출하고 싶을 때

<br/>
tasklet 클래스를 @ component 로 작성해보자  


```java
@Configuration
@RequiredArgsConstructor
public class ArchiveJobConfig {
	...
    @Autowired
    private StepBuilderFactory stepBuilderFactory;
    @Autowired
    private StorageService storageService;
  	...
    @Bean
    public Step archiverItemStep() {
        return stepBuilderFactory.get("archiverItemStep")
                .tasklet(new ArchiverTasklet(storageService))
                .build();
    }
}
---------------------------------------------------------------------------
@Component
public class ArchiverTasklet implements Tasklet {
    private StorageService storageService;
    public ArchiverTasklet(StorageService storageService){
        this.storageService = storageService;
    }
    @Override
    public RepeatStatus execute(final StepContribution stepContribution, final ChunkContext chunkContext) {
        storageService.archiver();
        return RepeatStatus.FINISHED;
    }
}

```


<br/><br/><br/>
  

   
## JobBuilderFactory 에서 if문 같은 on 메소드를 활용하고 싶은데, 조건에 걸린 next 메소드의 step이 tasklet 형식이 아닌 chunk 형식일 때
<br/>
- 상황 설명 :   

  - buildBuilderFactory의 next 메소드가 step 형식

```java
Job porterJob = jobBuilderFactory.get(rootFile.toString())
                            .incrementer(new RunIdIncrementer())
                            .start(syncerJobConfig.partitionScanItemStep())
                            .next(syncerJobConfig.partitionDetectItemStep())
                            .next(syncerJobConfig.removeItemStep())
                            .listener(jobListener)
                            .build();

```
  - 여기서 partitionDetectItemStep 의 결과에 따라 removeItemStep 이 결정되어야 하는 상황

```java
@Bean
    public Step partitionDetectItemStep() {
        return stepBuilderFactory.get("partitionDetectItemStep")
                .partitioner("detectSlaveStep", detectPartitioner(null, null, null))
                .step(detectItemStep())
                .taskExecutor(syncerThreadPoolTaskExecutor)
                .build();
    }

@Bean
    public Step removeItemStep() {
        return stepBuilderFactory.get("removeItemStep")
                .tasklet(removeItemStepTasklet(null, null, null))
                .build();
    }
```

* 방법 1 : JobExecutionDecider 인터페이스를 사용한 클래스 만들어 이용하기

```java
@Component
public class StatusDecider implements JobExecutionDecider {
    private static FlowExecutionStatus executionStatus;
    public void setExecutionStatus(){
        executionStatus = new FlowExecutionStatus("SUCCESS");
    }
    public void setFailExecutionStatus(){
        executionStatus = new FlowExecutionStatus("SPARK_EXCEPTION");
    }
    @Override
    public FlowExecutionStatus decide(JobExecution jobExecution, StepExecution stepExecution) {
        return executionStatus;
    }
}
```
```java
@Component
public class JobListener implements JobExecutionListener {
    @Autowired
    private StatusDecider statusDecider;

    @Override
    public void beforeJob(final JobExecution jobExecution) {
        statusDecider.setExecutionStatus();
    }

    @Override
    public void afterJob(final JobExecution jobExecution) {
		...
    }
}
```
```java
Job porterJob = jobBuilderFactory.get(rootFile.toString())
                            .incrementer(new RunIdIncrementer())
                            .start(syncerJobConfig.partitionScanItemStep())
                            .next(syncerJobConfig.partitionDetectItemStep())
                            .next(syncerJobConfig.checkStatusItemStep())
                            .from(syncerJobConfig.checkStatusItemStep())
                                .on("SPARK_EXCEPTION")
                                .to(syncerJobConfig.failedItemStep())
                            .from(syncerJobConfig.checkStatusItemStep())
                                .on("SUCCESS")
                                .to(syncerJobConfig.removeItemStep())
                            .end()
                            .listener(jobListener)
                            .build();
```
  - detectItemStep 의 step 에서 StatusDecider 를 @Autowired 하고 status를 바꿔준다.

<br/><br/>

* 방법 2 : jobListener를 이용하기
  - 상태를 공유하는 변수를 jobListener에 생성

```java
@Component
public class JobListener implements JobExecutionListener {
    private static ExitStatus exitStatus;

    @Override
    public void beforeJob(final JobExecution jobExecution) {
        exitStatus = new ExitStatus("EXECUTING");
    }

    @Override
    public void afterJob(final JobExecution jobExecution) {
        ...
    }

    public ExitStatus getExitStatus() {
        return exitStatus;
    }

    public void setExitStatus(ExitStatus exitStatus) {
        this.exitStatus = exitStatus;
    }
}
```
  - jobBuilderFactory에는 next만 사용한 기존 코드를 그대로
```java
Job porterJob = jobBuilderFactory.get(rootFile.toString())
                            .incrementer(new RunIdIncrementer())
                            .start(syncerJobConfig.partitionScanItemStep())
                            .next(syncerJobConfig.partitionDetectItemStep())
                            .next(syncerJobConfig.removeItemStep())
                            .listener(jobListener)
                            .build();
```

  - detectItemStep 의 step 에서 JobListener를 @Autowired 하고 status를 바꿔준다.
  - removeItemStep 에서 JobListener를 @Autowired 하고 ExitStatus에 따라 제어한다.

```java
@Bean
    public Step removeItemStep() {
        return stepBuilderFactory.get("removeItemStep")
                .tasklet(removeTasklet(null, null, null))
                .build();
    }

    @Bean
    @StepScope
    public Tasklet removeTasklet(@Value("#{jobParameters[mainPath]}") String mainPath, @Value("#{jobParameters[resultPath]}") String resultPath, @Value("#{jobParameters[sequence]}") Long sequence) {
        return (stepContribution, chunkContext) -> {
            if (jobListener.getExitStatus().getExitCode().equals(Constants.DETECT_ITEM_EXCEPTION)) {
                ...
            } else {
                ...
            }

            return RepeatStatus.FINISHED;
        };
    }
```





