# 호튼웍스 설치하기

### < 목표 >
1. 호튼웍스가 무엇인가
2. 암바리 설치
3. 호튼웍스 설치
5. 호튼웍스 툴 만져보고 활용 가능성에 대해 생각하기

<br/>
reference : https://kyumdoctor.tistory.com/54?category=878638
<br/><br/><br/><br/><br/>

### 1. 호튼웍스가 무엇인가?

레퍼런스에 따르면 다음과 같이 설명이 있다.  

<img src="https://github.com/kimhagyeong/Tech_Diary/blob/main/static/%E1%84%92%E1%85%A9%E1%84%90%E1%85%B3%E1%86%AB%E1%84%8B%E1%85%AF%E1%86%A8%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%86%E1%85%A7%E1%86%BC.png"
     width="700"/>


Hortonworks에선 상호 운용 가능한 세 가지 제품 라인이 있다.

Hortonworks Data Platform (HDP) : Apache Hadoop , Apache Hive , Apache Spark 기반
Hortonworks DataFlow (HDF) : Apache NiFi , Apache Storm , Apache Kafka 기반
Hortonworks DataPlane 서비스 (DPS) : Apache Atlas 및 Cloudbreak 및 IBM 과 같은 파트너 가 서비스를 추가 할 수 있는 플러그 형 아키텍처를 기반.

호튼웍스는 설치시 암바리를 사용해야 한다. 
기본적으로 하둡은 리눅스 환경에서 진행되어야 하기 때문에 vm을 생성해서 테스트해야 한다.  
나는 사내 서비스 centos vm instance를 생성해서 진행했다.  
물론 사내 vm 이기 때문에 root 명령어를 사용하려면 sudo -i 로 로그인하고 logout을 쳐서 로그아웃 하자.
<br/><br/>
#### Ambari?
Hadoop eco 설치, 설정배포, 모니터링, Alert 등의 운영 편의성을 제공하는 오픈소스 관리 시스템 툴. 
Ambari나 Cloudera Manager와 같은 운영툴이 없던 시절에 Hadoop 설치는 여간 까다로운 것이 아니었으며, 모니터링도 상당히 번거로웠다.  
사실 그 당시만 해도 하둡 설치할 수 있다는 것만 해도 상당한 Know-how 였으나, 최근에는 이런 툴들이 보급 되면서 설치나 운영도 과거에 비해 상당히 장벽이 낮아졌다.  
출처: https://datacookbook.kr/32 [DATA COOKBOOK]  
출처: https://bryant.tistory.com/105?category=584415 -> centos 기반 HDP설치 가이드
  
1. centos 에서 java 설치
<pre><code>
$ sudo yum install java-1.8.0-openjdk
# 안해도 잘되긴 함
$ vi ~/.bashrc
i -> [insert mode]
#---
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.292.b10-1.el7_9.x86_64/jre
export PATH=$PATH:$JAVA_HOME/bin
#---
#추가하고 esc + :wq 하면 저장하고 종료
$ source ~/.bashrc
# 갑자기 ls 같은 명령어를 못 찾을 수 있음.그럴 때는
$ vi ~/.bashrc
#---
export PATH=$PATH:/bin:/usr/local/bin:/usr/bin
#---
$ source ~/.bashrc
</code></pre>
  
  
2. mysql 설치
centos에는 기본적으로 mariaDB가 있다고 한다.  
하지만 설치한 설정마다 다를 수 있다.  
나의 경우에는 yum list installed ma(탭 두번) 하면 ma 로 시작하는 인스톨된 yum 리트르를 받아와봤는데 여기에 mariaDB가 없었다.  
mariaDB가 있으면 제거하고 mysql을 설치하면 된다.  (https://sailer.tistory.com/entry/Centos-7-%EC%97%90-MYSQL-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B056-or-57)

<pre>
<code>
# MySql 5.6 버전
 $ sudo yum -y install http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm 
 $ sudo yum install mysql-community-server 
# 시작프로그램 등록 (하기전 sudo -i 를 해야할 수도 있다. 이건 Root 환경으로 가기 위함이다.)
 $ systemctl enable mysqld
 $ systemctl start mysqld
# 시작프로그램 종료
 $ systemctl stop mysqld
# mysql status
 $ systemctl status mysqld
</code>
</pre>
기본적으로 https://bryant.tistory.com/105?category=584415 -> centos 기반 HDP설치 가이드 요 출처 따라서 하면 되는데, 
#/usr/bin/mysql_secure_installation
이게 안되는 경우  
#sudo mysql_secure_installation  
을 쳐보자.  
그 외, 안 될 때는 sudo 명령어를 쳤는지, root 환경인지 확인한다.
