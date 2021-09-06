## 하둡과 서버는 리눅스! 리눅스 명령어 리뷰

``The Linux Command Line 책 리뷰!``

- ls -l

"-l"은 디테일하게 보겠다는 뜻

- cd ~

"~" 는 홈디렉터리

- ls -lt

"l" 은 디테일하게 보겠다, "t"는 옵션과 파일의 수정 시간을 기준을 나열하겠다. 합쳐서 "-lt" 디테일하게 시간순으로

- ls -lt --reverse

mac에서는 "-reverse", 단간하게 "r"만 쓸 수 있음

- less ha.txt

내용 출력. exit less 하려면 press "q"  
G면 마지막 위치, g는 첫 위치  
b면 page up, 스페이스는 page down
간단하게 보고 싶으면 cat 명령어 사용

- root, boot, bin, etc

https://wlgnschlkkk.tistory.com/62

- cp : Copy files and directories  
- mv : Move/rename files and directories  
- mkdir : Create directories  
- rm : Remove files and directories   
- ln : Create hard and symbolic links  


about whildcard

- ``*`` : All files
- g* : Any file beginning with “g”
- b*.txt : Any file beginning with “b” followed by any characters and ending with “.txt”
- Data??? : Any file beginning with “Data” followed by exactly three characters
- [abc]* : Any file beginning with either an “a”, a “b”, or a “c”
- BACKUP.[0-9][0-9][0-9] : Any file beginning with “BACKUP.” followed by exactly three numerals
- [[:upper:]]* : Any file beginning with an uppercase letter Any file not beginning with a numeral
- [![:digit:]]* : Any file not beginning with a numeral
- *[[:lower:]123] : Any file ending with a lowercase letter or the numerals “1”, “2”, or “3”


- cp item... directory

to copy multiple items (either files or directories) into a directory.  
cp 옵션은 -a, -r, -i, -u, -v 등이 있는데, 주로 -r 을 많이 사용한다.  
-r 은 recursive 라는 뜻으로 디렉토리를 복사할 때 사용한다.  
옵션들에 대한 자세한 내용은 "the linux command line" 53page 참고. 



" > "  리다이렉션 기호

- ls -l /usr/bin > ls-output.txt  
명령어 결과 값을 다음과 같이 저장한다는 의미.
- ls -l /usr/bin >> ls-output.txt  
이미 파일이 존재한 경우 결과값은 기존 데이터에 append.

" | " 파이프 기호
- ls -l /usr/bin | less  
앞 명령어의 결과를 뒤에 나오는 명령어의 입력으로 처리하기 위해 사용하는 방법


- echo $((2 + 2))  
$(())를 사용해서 Arithmetic표현할 수도 있다.


Permissions

- id – Display user identity  
- chmod – Change a file's mode  -> owner group world
- umask – Set the default file permissions  
- su – Run a shell as another user  
- sudo – Execute a command as another user  
- chown – Change a file's owner  
- chgrp – Change a file's group ownership  
- passwd – Change a user's password  



Processes


- ps – Report a snapshot of current processes. 
- top – Display tasks. 
- jobs – List active jobs. 
- bg – Place a job in the background. 
- fg – Place a job in the foreground. 
- kill – Send a signal to a process. 
- killall – Kill processes by name. 
- shutdown – Shutdown or reboot the system. 

(작성중..)
