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
#### Ambari?
Hadoop eco 설치, 설정배포, 모니터링, Alert 등의 운영 편의성을 제공하는 오픈소스 관리 시스템 툴. 
Ambari나 Cloudera Manager와 같은 운영툴이 없던 시절에 Hadoop 설치는 여간 까다로운 것이 아니었으며, 모니터링도 상당히 번거로웠다.  
사실 그 당시만 해도 하둡 설치할 수 있다는 것만 해도 상당한 Know-how 였으나, 최근에는 이런 툴들이 보급 되면서 설치나 운영도 과거에 비해 상당히 장벽이 낮아졌다.  
출처: https://datacookbook.kr/32 [DATA COOKBOOK]

