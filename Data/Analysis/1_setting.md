1. anaconda. 
아나콘다 웹 -> mac package 다운로드, 설치  
cmd 창에서   
$conda creat -n test anaconda
아나콘다는 vm 환경  
위 명령어는 vm 인스턴스 생성  
뒤에 anaconda 명령어는 안 쓸 경우 너무 빈 인스턴스가 생성되기 때문
$conda activate test 
$conda deactivate

2. jupyter notebook 열기
cmd 창에서   
$conda activate test  
$jupyter notebook  
chrome default 창이 열림  
창이 열려도 본체는 cmd 창. 즉 cmd 창을 닫으면 안됨  
주피터 노트북은 노트북 말그대로 문서를 의미할 수 있는데, 코드와 마크다운을 동시에 표현할 수 있는 문서 환경이다.   
jupyter notebook 브라우저는 현재 나의 피시를 보여주고, 여기서 .jpynb 확장자의 파일을 열면 문서를 overview 할 수 있다.
"셀 cell" 단위로 작업이 진행  
마크다운 셀과 코드 셀이 있음.  
esc 누르고 m 누르면 마크다운 셀, 셀 생성시 기본 셀은 코드 셀  
shiftt+enter 를 치면 실행이 됨

3. Colab
구글에서 제공해서 바로 쓸 수 있는 jupytter notebook 느낌이 colab.  
웹에서 제공하는 것이기 때문에 (클라우드 컴퓨팅 개념) 로컬에서 작동되는 쥬피터 노트북과 퍼포먼스가 다름..
현재는 파이썬 포함해서 구글에서 제공하는 공유 기능을 제공.  






