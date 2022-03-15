#### 문제 : 서버에서 코드를 돌리면 hs_err_pid.log가 생성되면서 프로그램이 종료됨

<img src="https://github.com/kimhagyeong/Tech_Diary/blob/main/static/fatal%20error.png" width="700"/>    

에러 로그 분석 방법 : https://docs.oracle.com/javase/7/docs/webnotes/tsg/TSG-VM/html/felog.html    
core dump는 ulimit -c 가 0일 경우 core dump 에러문을 생성할 수 없어서 위와 같이 unlimit -c unlimited를 설정해달라는 의미    
<br/>
##### 안 1) 로그 파일 중에서 signal name과 frame type 을 기준으로 해결 방법 모색.   
<br/>
signal name : SIGBUS

signal name 중 SIGBUS와 SIGSEGV 에러 차이 :

( 참조 : https://sleekdev.tistory.com/40 )      

이 시그널은 유용하지 않은 포인터가 비참조되었을 때 발생 

두 시그널은 모두 초기화되지 않은 포인터를 비참조 한 것의 결과이다.

SIGSEGV는 유용한 메모리에서 유용하지 못한 억세스를 지적하고,

SIGBUS는 유용하지못한 주소를 억세스 하는 것을 지적한다. 

특별하게, SIGBUS 시그널은 4개로 나누어지지 않은 주소에 4-단어 정수로 참조하는것처럼, 부적당한 포인터가 비참조 됨으로써 발생한다.

(각종 시스템은 주소 정렬은 위한 자신만의 필요조건을 갖는다. )

이 시그널의 이름은 "bus error"의 약자이다.

<br/>
SIGBUS 발생 원인 :

1. java가 live 한 상태에서 jar 파일 같은 파일이 변경/업데이트 되는 경우


===============================================================     
This is a common issue where a process or thread has modified the contents of a jar/zip file while it has been mmap'ed into memory by the JDK process. 

The JDK cannot protect against such action without a performance impact. The JDK mmaps in the central directory on zip files on Linux and Solaris OSes.   

Runing JDKs with -Dsun.zip.disableMemoryMapping=true system property will disable memory mapping and the application will fail instead with a "file corrupted" type ZipException.

Marking this incomplete. Submitter will need to determine who/what is updating the jar/zip file that's causing issue.  

===========================================================================        

<br/>
2. /tmp가 full이 날 때 "VM crashes with SIGBUS writing PerfData if tmp space is full"


-> 하지만 /tmp 용량은 여유로웠다.

<br/><br/>

##### 안 2) frame type : C 

C 는 Native C frame에서 문제가 났다고 이야기하고 있다.

동일한 frame type 에러가 난 블로그를 보면

1. BIGBUS 에러의 1번과 동일 
2. 기기의 SWAP 공간이 부족   


문제가 날 수 있다고 말해준다. jar 변경은 일어나지 않았을 테니, 메모리 쪽에서 부족이 일어나는 것으로 생각이 들었다.

스레드에서 GCTaskThread 에러가 나는 것으로 보아 가비지 컬렉션 쪽에 문제가 나는 것으로 생각했고 이에 gc를 해결한 블로그를 참고했다.

<br/>
gc 수행 오류로 인한 jvm 오류 해결 : https://d2.naver.com/helloworld/1134732

gc, eden, survivor 설명 : https://12bme.tistory.com/57

진행 중인 해결 방법 : JAVA 실행 옵션에 -XX:-ReduceInitialCardMarks 추가

출처: https://eat-hokey.tistory.com/49 [Hokey]
<br/>
해당 출처에서 core jump는

JAVA는 GC(가비지 컬렉션) 수행 중 쓰레기로 판단된 쓰레드에 marking을 진행하는데 이때 걸리는 시간이 오래 걸리면서 나타나는 현상.

즉, 쓰레기로 판단된 쓰레드를 반환하면서 메모리에 공간단편화가 진행 될 때 버그가 발생한다고 한다.

공간 단편화로 프로세스를 넣을 주소가 없어서 SIGBUS가 난다고 판단했고 블로그에 올려진 대로 java 실행 때 위 옵션을 추가해서 돌려보았다.


<br/>

하지만 이 역시 해결되지 않았다.
<br/><br/><br/>


##### 안 3) 디스크 문제

/var/log 위치에서 ls -al 명령어를 통해 디스크 장애를 확인할 수 있다.
아래 스크린샷과 같이 디스크 장애가 없는 경우에는 total data 가 잘 출력되지만

그렇지 않는 경우에는 에러가 난다.

해당 문제는 서버 관리 팀에 문의하여 해결하였다.

<img src="https://github.com/kimhagyeong/Tech_Diary/blob/main/static/%E1%84%83%E1%85%B5%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20O.png" width="400"/>   
<img src="https://github.com/kimhagyeong/Tech_Diary/blob/main/static/%E1%84%83%E1%85%B5%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%20%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%A2%20X.png" width="400"/>   
