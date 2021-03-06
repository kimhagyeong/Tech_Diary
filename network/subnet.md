
### 1. 시작

서브넷을 찾아보게 된 이유는, Azure vm을 생성했지만 인터넷 접속을 위해서 포트 허용이 필요했고,    
ip 를 일일이 포트오픈 신청하는 것보다 서브넷을 통해 표현하는 것이 편리했기 때문이다.

회사에서 Azure vm의 ip 가 192.12.0.155 라고 가정을 했을 때 이 Ip 의 서브넷은 192.12.0.128/27 이라고 한다..   
저 표현은 어떤 표현인지, 저 표현으로 Azure vm ip 를 포함할 수 있는지 알아보조가 한다.


### 2. subnet, subnet mask, cidr

서브넷 : 부분 네트워크   
mask : mask 연산 = AND 연산    
11010110 에서 뒷자리 4개를 0000 으로 만들고 싶다면?    
11110000 AND 연산을 해준다   
= 11010000    



그럼 서브넷은 왜 하나?    
회사 단위에서는 ip 를 C class 로 할당하게 되면, C class는 컴퓨터(host)가 256개(실제는 -2 = 254개) 있다는 의미가 된다.    
회사에 따라 254개의 컴퓨터는 너무 많을 수 있으니, 이 것을 두 세 부류로 나누어 사용하고 싶을 수 있다.    

이럴 때 Mask 를 이용해서 네트워크를 분리하는 것이 subnet mask 이다.

<br/><br/>
subnet mask 를 표현하는 방식은 ip 표현과 흡사해서 ip 주소와 헷깔리면 안된다.    
아래와 같은 ip 주소가 있고
```
205.0.1.129
```
subnet mask 가 아래와 같이 있다고 하면
```
255.255.255.0
```
네트워크 주소 범위는 205.0.1.0 ~ 205.0.1.255 이 된다.    
C class 라고 했으니 맨 마지막 8비트는 host 가 된다고 할 때   
AND 연산이라고 했으니 아래와 같은 과정을 거치게 되는데,
```
    11001101 00000000 00000001 xxxxxxxx
AND 11111111 11111111 11111111 00000000
---------------------------------------
    11001101 00000000 00000001 00000000
    205 .    0 .      1.       0
```
이 주소는 어떤 ip가 들어와도 마스킹 이후 결과가 동일하기 때문에 네트워크가 동일하다는 의미.    
마스킹 이후 결과가 다르다면 네트워크가 분리 된다는 것.    


네트워크를 2개로 분할하려면 ?   
subnet mask
```
255.255.255.128
```
```
    11001101 00000000 00000001 xxxxxxxx
AND 11111111 11111111 11111111 10000000
---------------------------------------
    11001101 00000000 00000001 00000000
또는 11001101 00000000 00000001 10000000

    205 .    0 .      1.       0
또는 205 .    0 .      1.       128
```
이렇게 결과가 두 가지가 나오는 것은, 네트워크가 2개로 분할되었다는 의미.    
그렇다면 255.255.255.128 은 앞에서 25개의 비트는 네트워크 주소라는 것을 알 수 있고   
그 범위는 205.0.1.0 ~ 205.0.1.128 , 205.0.1.129 ~ 205.0.1.255 이라는 것을 알 수 있다.


<br/><br/>
여기서 멈추지 않고 이러한 범위를 간소화 해서 표현할 수 있는데,   
앞서 25개의 비트는 네크워크 주소라는 의미로 /25 로 표현하고    
129부터 시작하는 범위라는 의미로 아래와 같이 표현한다.
```
205.0.1.129/25
```

이를 cidr 표현이라고 한다.


<br/><br/>

### 3. 증명
Q. 192.12.0.128/27 의 네트워크 범위에 192.12.0.155 가 포함되는가?   


먼저 192.12.0.128 를 2진수로 표현해보자
```
11000000. 00001100. 00000000. 10000000
```
앞에서 27개의 비트는 네트워크 주소
```
11000000. 00001100. 00000000. 100xxxxxx
```
host 개수는 5자리 
```
11111
```
이를 10진수로 변형했을 때 값은 31    
192.12.0.128/27 의 네트워크 범위는
```
11000000. 00001100. 00000000. 10000000
 ~ 11000000. 00001100. 00000000. 10011111
----------------------------------------
192.12.0.128
 ~ 192.12.0.159
```
First Ip = 192.12.0.128   
Last Ip = 192.12.0.159    
범위이기 때문에 192.12.0.155 는 해당 네트워크 범위에 포함된다!
