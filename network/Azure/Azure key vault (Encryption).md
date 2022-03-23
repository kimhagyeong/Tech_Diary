
### 1. Azure key vault
Azure 에서 기본적으로 제공하는 암호화 서비스가 있다.   
private end point 를 이용해서 암호화 key 값을 가져오는데,    
이 암호화 key 값을 이용해서 blob 을 업로드할 때 암호화를 하고, 암호값을 Metadata 에 저장한다.   

동일한 key 값을 사용해서 blob을 download 하지 않으면 데이터를 읽을 수 없다.



이렇게 강력한 암호화를 제공해주고 있는데 RSA 방식을 사용한다.   

### 2. RSA
기본적으로 암호화는 크게 양방향 단방향 방식이 있다. 
```
                  __ 대칭키 -> AES
       __  양방향 ㅡ|
      |           |_ 비대칭키 ( 공개키, 개인키 ) -> RSA
암호화ㅡ|
      |__  단방향 - HASH
```
RSA 특징 
- 인수분해를 이용한 암호화 기법이다 
- 디지털 서명등에 사용된다
- 인수분해를 이용하기 때문에 공개키가 짧다는 단점이 있다.
- 보통 스피닝과 같은 노출 위험이 있을 때 방지하기 위해 사용된다.
- 공개키가 짧기 때문에 공개키를 다시 대체하는 대칭키 AES 를 사용한다.

시나리오

```
                   RSA 서버 생성
                        |
            ----------------------------------------
           |                                        |
      공개키 R 생성                               개인키 R 생성
------------------------------------------------------------------
           |                                        |
           |                                        |
 클라이언트(데이터 저장을 요청한 쪽) 사용        본 서버(데이터 저장하는 쪽) 사용
           |                                        ^
           |________________________________________|
      클라이언트가 공개키 R 을 사용해서 데이터 암호화 요청을 본 서버로 보냄

```
암호화 과정   
1. 사용자가 직접 AES 대칭키를 생성
2. AES 대칭키를 이용해서 데이터를 암호화
3. AES 대칭키를 RSA 공개키로 암호화
4. 암호화된 데이터를 Azure storage 에 업로드 + 암호화된 AES 대칭키도 메타데이터에 매핑되어 저장
<br/><br/>

복호화 시나리오
1. RSA 개인키로 메타데이터에 있는 암호화된 AES 대칭키를 해독
2. 해독한 대칭키로 데이터 복호화
3. 복호화한 데이터 전송!

참조 : https://docs.microsoft.com/ko-kr/azure/storage/common/storage-client-side-encryption-java?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=java


### 3. key vault endpoint
java 에서 암호화를 하려면 아래와 같은 azure api 를 사용한다.
```
ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(azureClientId, azureTenantId, azureClientSecret, AzureEnvironment.AZURE);
            KeyVaultClient keyVaultClient = new KeyVaultClient(credentials);
            KeyBundle key = keyVaultClient.getKey(azurePrivateEndpoint, azureKeyName);

            KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(keyVaultClient);
            IKey iKey = cloudResolver.resolveKeyAsync(key.keyIdentifier().toString()).get();

            // Set the encryption policy on the request blobRequestOptions.
            BlobEncryptionPolicy policy = new BlobEncryptionPolicy(iKey, null);
            BlobRequestOptions blobRequestOptions;
            blobRequestOptions.setEncryptionPolicy(policy);
```
key vault 에 접근하기 위해서는 Private endpoint 사용하는데,   
private endpoint 를 만들기 위해선 Azure portal 에서 준비 과정이 필요하다.

1. Azure Active Directory 에 앱을 등록한다.  
    -> Client ID를 얻기 위함이다.
2. 키 자격 증명 모음 > 키 > 만들기 RSA 형식의 key 를 생성한다.
    -> 생성한 key 이름은 Azure key name을 얻기 위함이다.
3. 키 자격 증명 모음 > 만들기
    -> 액세스 정책 설정
    -> 네트워크 제한
4. 스토리지 > 네트워킹 > 프라이빗 엔드 포인트 연결 > 만들기
    -> Azure private endpoint 를 얻기 위함이다.


<br/><br/><br/>


#### 참고  
- CEK : 사용자가 직접 사용, 정보를 암호화 하는 키, AES 대칭키
- KEK : 키를 암호화하는 키, RSA 공개키
- BlobRequestOptions 은 암호화된 대칭키이다.
