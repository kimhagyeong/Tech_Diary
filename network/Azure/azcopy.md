
azure 에는 몇가지 data migration tool들이 있다.  
https://docs.microsoft.com/en-us/azure/storage/common/storage-choose-data-transfer-solution?toc=/azure/storage/blobs/toc.json  
<img src="https://github.com/kimhagyeong/Tech_Diary/blob/main/static/azcopy.png" width="700">

bandwith, data 크기에 따라 data migrate 툴을 제안해주는데, 이 중 편리하게 사용 가능한 azcopy를 통해 data migration을 진행하였다.  
- large dataset, low network bandwith : Azure Data Box family for offline transfers
- large dataset, high network bandwith : AzCopy , Azure Storage REST APIs/SDKs, Azure Data Box family for online transfers, Azure Data Factory
- small datasets limited network bandwith : Azure Storage Explorer , Azure portal , AzCopy/PowerShell/Azure CLI and Azure Storage REST APIs

<br/><br/>
#### azcopy test 중 알아낸 특징
1. container가 없으면 생성해서 copy 한다
2. 동일한 파일이 목적지에 존재하면 덮어쓰기 되지 않고 스킵 된다.
3. dataset 에 비해 bandwith가 크지 않으면 과부하 발생
4. 추가적인 로그인 없이 tenant 가 달라도 이관 가능하다.  
```
azcopy 명령을 실행할 때 스토리지 url 을 사용하는데  
(예 :azcopy cp "https://blabla.blob.core.windows.net/service/{SAS}" "https://blabla.blob.core.windows.net/service/{SAS}" )
이 url 은 모든 계정에서 유니크하고, SAS 토큰을 매번 생성해서 이용하기 때문에 가능하다. 

계정마다 테넌트 간 복제 허용을 하는 속성도 있고  
SAS 토큰 생성시 서비스, 리소스 등을 직접 허용하는 절차가 있기 때문에  
테넌트 로그인을 하지 않아도 신뢰하고 이관할 수 있다.


```


→ azcopy를 사용한다면 대용량 컨테이너는 migrate 제외하고 container 별로 진행하는 것이 좋아 보인다..

<br/><br/>

### azcopy 사용하기

azcopy 사용하려면 azcopy 프로그램을 다운받고, 다운받은 위치를 환경변수에 추가해야함.  
cmd 에 azcopy 를 쳤을 때 잘 출력되면 준비 완료.  
<br/>
초기에 tenant id로 로그인을 하면 access token이 .azcopy 폴더 안에 생성되면서 이후부터는 테넌트 로그인을 하지 않아도 된다.


1. azcopy 다운받고 설치  
    - https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/blobs/toc.json → Windows 64-bit → 설치 후 압축풀고 환경변수 변경  
    - C:\ 상단에 AzCopy 폴더 만들고 그 안에 azcopy.exe 파일을 이동
    - 환경변수 > 시스템변수 > Path 클릭 후 편집 > C:\AzCopy 추가  
    - cmd 창에 azcopy 쳐보기
2. 테넌트 및 기본 설정
    - azcopy login --tenant-id={tenant-id}
3. SAS 생성
    - account > 공유 액세스 서명 에 SAS 토큰 받아옴
    <img src="https://github.com/kimhagyeong/Tech_Diary/blob/main/static/azcopy%20sas.png" width="700" />
    - 허용 범위에 따라 체크 후 SAS 생성
4. azcopy get start를 한 후 아래 copy를 진행하여 container, blob, directories를 모두 옮긴다. 
```
azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/' --recursive
```

<br />
마이그레이트 소요 시간은 pc 성능에 따라 다르다.    

나의 경우, bench 옵션을 사용하여 소요 시간을 예측했고, 실 소요 시간은 bench 테스트의 2배정도 걸렸다.  
1TB에 한시간 정도 소요되었다.


### 업로드 실패 시 시나리오
- 실패 원인은 크게 두 가지로 예상
    - blob 명과 path 명이 동일할 때   
        - /playground/test/test blob과 /playground/test/test 폴더가 있을 경우

    - scan 한 데이터가 삭제된 경우
<br/><br/>
- upload failed 발생 시
    - log 모음에서 DOWNLOADFAILED 검색해서 실패한 blob의 url을 storage explorer로 찾음
    - 복사 → new storage account 에 동일한 path에 붙여 넣기
