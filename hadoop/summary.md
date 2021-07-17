클라이언트로 부터 받는 로그들을 하둡에 저장.

on premise 환경에서의 하둡 저장 : 사내 인프라. 직접 설비한 서버, 하드웨어 등을 의미. 데이터가 부족하게 되면 이것 저것 연결해놓은 아키텍처들을 다 늘려야하기 때문에 확장하기 어려움.
public cloud에서의 하둡 저장 : azure와 aws 같은 클라우드에서는 손쉽게 사이즈를 늘릴 수 있음.

데비안, 페도라, 우분투와 같은 리눅스를 만든 회사들이 있고 아파치 하둡과 같이 하둡 오픈 소스들이 있다.
이를 패키징해서 상용하는 회사 red hat을 사용하고 있고 아파치 하둡을 패키징 하는 회사 hortonworks가 있다.

우분투도 리눅스를 패키징하는 회사인데 red hat과 차이점이 있다면 사용목적이 다르다. 우분투는 여러 기능을 가지고 있어 손쉽게 어플리케이션을 배포할 수 있고 레드햇은 보안에 초점을 맞추고 있다.
https://m.blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=ki630808&logNo=221895883311&categoryNo=62&proxyReferer=

Hortonworks에선 Hortonworks에는 상호 운용 가능한 세 가지 제품 라인이 있다.
- Hortonworks Data Platform (HDP) : Apache Hadoop , Apache Hive , Apache Spark 기반
- Hortonworks DataFlow (HDF) : Apache NiFi , Apache Storm , Apache Kafka 기반
- Hortonworks DataPlane 서비스 (DPS) : Apache Atlas 및 Cloudbreak 및 IBM 과 같은 파트너 가 서비스를 추가 할 수 있는 플러그 형 아키텍처를 기반. 
호튼웍스 적용 example : https://kyumdoctor.tistory.com/54

눈에 익은 프레임워크들이 많은데 apache hadoop, apache hive, apache spark, apache kafka 등. 앞으로 이 툴을 사용해볼 예정.
호튼웍스에서 지원하는 기능들을 자체 제작할 수도 있음.

하둡에 저장되어 있는 분산 데이터들을 일반 개발자들도 보고 싶을 때 sql 형식으로 부를 수 있도록 제공한 오픈 소스 서비스가 hive.
Hue는 다양한 하둡 서비스, HDFS 브라우저를 UI로 지원한다.
물론 hue에 hive를 연동할 수 있다.
Hue VS HIVE
Hue : 웹 인터페이스로 편리함  주로 클라우데라 하둡에서 쓰임, 클라우데라 최적화 
Hive : SQL 인터페이스. DML, DDL DCL 등 쿼리로 HDFS 관리를 함.
https://teruto.tistory.com/5

difference : https://www.geeksforgeeks.org/difference-between-hive-and-hue/

spark 는 분산처리. IDE 환경에서 사용 가능하도록 제공함. (visual code, intelliJ)

Apache Zeppelin은 Spark를 통한 데이터 분석의 불편함을 Web기반의 Notebook을 통해서 해결해보고자 만들어진 어플리케이션이다.
https://medium.com/apache-zeppelin-stories/%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4-%EC%9D%BC%EA%B8%B0-2-apache-zeppelin-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-f3a520297938

 
 
위 내용을 바탕으로 가상환경에 직접 하나씩 툴들을 테스트 해보자!
