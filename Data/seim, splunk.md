### 1. SEIM, Splunk intro

SIEM은 Security information and event management 의 약자로 통합보안관제 시스템이다.  
사내에서 보안 취약점을 예방하고자 모니터링 시스템 도입이 필요하게 되었고 elastic search, splunk 와 같이 분석 플랫폼을 siem에 도입하는 것을 시도하게 되었다.
따라서 SIEM은 보안관제시스템을 지칭하는 용어이고, splunk는 그 솔루션 중 하나인 것이다. <br/>

splunk는 보안 기능에 대해 강조가 많이 되는 편인 듯 하나, 원래는 데이터 분석 플랫폼이다.
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







