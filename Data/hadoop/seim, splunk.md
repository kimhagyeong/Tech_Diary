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
로그 수집을 위한 컴포넌트, 보통 indexer로 데이터를 전송하는 역할    
원시 데이터를 전송    
각 보안 장치들에서 


