# Nginx
vs Apache http server 와 비교하면 좋다.    
- 필수 시청 : https://www.youtube.com/watch?v=6FAwAXXj5N0&list=WL&index=41
- apache 서버는 이전 웹 서버의 버그들을 수정하기 위해 나온 서버로 (os)호환성, 확장성이 좋다. 
- apache 서버는 client 들의 요청을 connection으로 연결하고 그걸 각각 process 로 처리함. 트래픽이 많아지는 시점에서 context switch 문제가 다량 발생
- 이를 극복하기 위해 connection으로 들어오는 일들을 worker process에 큐 형태로 쌓고, 오래 걸리는 job에 대해서 thread 를 처리함으로써 동시 커넥션 문제를 해결한게 nginx
- 초기에 nginx는 apache 서버 앞단에 설치함으로써 트래픽 한계를 해결하려는 목적으로 만들어졌지만, 그 자체로 웹 서버가 가능함.
- 현재는 아파치 서버도 트래픽 문제를 해결하기 위해 개선이 되었고 ( 그럼에도 퍼포먼스는 Nginx 가 더 좋음) nginx도 트래픽 이외에 다양한 기능들을 제공한다.
- 나의 경우, AWS 인스턴스 서버에서 백엔드와 프론트 엔드의 https 적용을 위해서 쓰거나 cros 를 도입하기 위해 사용. 
- 이와 같이 각 웹서버의 기능을 찾아보고 니즈에 따라 선택
