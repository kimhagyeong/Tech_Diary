### 0. SEIM, Splunk intro

사내에서 seim 도입을 위해 splunk 설치를 진행하고 있다.  
splunk는 seim의 상위 개념이라고 이해했다. 
사내에서는 외주를 통해 설치를 진행하고 있는데, 외주 사이트에서 seim 과 splunk에 대해서 자세히 설명해주고 있어서 간단히 컨셉을 이해하기에 좋다.   
- SEIM : 
  - 공식 문서 : https://www.splunk.com/en_us/data-insider/what-is-siem.html
  - SIEM 솔루션, Enterprise Security :http://www.lint.co.kr/22

- Splunk :
  - 공식 문서 : https://www.splunk.com/en_us/about-us/why-splunk.html
  - SIEM에서 처리하지 못하는 데이터까지 모조리 수집하고 분석할 수 있다는 내용 : http://www.lint.co.kr/19 

- What Is the Concept of Splunk with SIEM? : https://www.comodo.com/is-splunk-a-siem.php



<br/><br/>



### 1. SEIM vs Splunk
- SEIM : data insight 를 얻기 위해 사용하는 GUI 인터페이스로 편리하고 다양한 기능들을 손쉽게 사용
- Splunk : siem 보다 자율성을 제공함으로써 siem에서 검색하기 어려운 보안 부분에 대한 트래픽도 직접 작성하고 모니터링 할 수 있음.    
보안 기능에 대해 강조가 많이 되는 편인 듯 하나, 원래는 데이터 분석 플랫폼임.    
각종 장비에서 발생하는 데이터를 형태와 상관없이 수집하고 저장.    
    
    
    
<br/><br/>



### 2. Splunk component
splunk 설치를 위해서는 아래 component 들에 대한 이해가 필요하다.    
필수 참고 : https://minloyal.tistory.com/entry/Splunk-Splunk-%EC%9D%98-%EA%B8%B0%EB%8A%A5%EA%B3%BC-%EC%93%B0%EC%9D%B4%EB%8A%94-%EC%9A%A9%EC%96%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-%EA%B8%B0%EB%B3%B8%ED%8E%B8?category=927998    
spunk 는 검색, 저장, 배포, 수집 4가지 큰 기능을 담당한다. 
- Forwarder : 
로그 수집을 위한 컴포넌트. 다른 forwarder나 indexer로 데이터를 전송하는 역할    

  - Universal forwarder : 
target 장비에서 발생하는 로그를 다른 forwarder나 indexer로 전송하는 forwarder    
로그를 수집하자마자 전송하기 때문에 filter 기능이 없음 ( raw data 그대로 전송 )    
endpoint 나 리눅스 서버 등, target 장비에서 설치될 수 있지만, target 장비가 많아지면 관리가 어렵기 때문에 장비들을 관리하는 서버 deployment server(manager server) 에 설치, 관리, 배포 됨.
  - Heavy forwarder :
target 장비 ( 보안 장비, 방화벽 장비 등 ) 에서 발생하는 로그들을 indexing(로그에 맞게 Indexer 로 매핑), filtering 해서 indexer 로 전송        
ex ) 방화벽 장비등에서 heavy forwarder를 바라보게 하고 UDP 로 전송~.  
다양한 connector 도 함께 설치 가능 (mysql, mssql 과 같이 db 쪽 데이터를 가져오거나 kafka 에서 데이터를 받을 수 있음.

    
    
- Indexer :
검색을 실제로 수행하고 search header로 반환.    
인덱스를 생성하여 데이터 인덱싱 및 관리, 저장, 수행    
하나의 인덱스에는 여러 포맷의 로그가 적재 가능.   
단위 : Bucket ( tier : hot, warm )

    
    
- SearchHeader :
최종적으로 검색 단말 역할, 사용자에게 검색 결과를 UI로 제공    
여러 Indexer에 검색을 명령하고 사용자에게 결과를 리턴함.    







