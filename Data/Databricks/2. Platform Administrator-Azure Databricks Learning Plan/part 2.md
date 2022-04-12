전반적으로 part 2 에서는 Azure Databrick 에서 제공하는 기능들과, 설정하는 방법을 admin 관점에서 안내해주고 있다.        
(사실 사내에서는 이미 설정이 완료된 부분...).   
관리자 학습과정이다 보니... 활용방법은 다음 학습에서..!

<br/><br/><br/>


# 1. Azure Databricks Cloud Architecture and System Intergration Fundamentals

### 1. Azure Databricks Reference Architecture
<img src="https://databricks.com/wp-content/uploads/2020/03/ETL-on-Azure_01.jpg" width="700px">
<img src ="https://miro.medium.com/max/1400/1*ij-beagXlQW-RRk3DyR_Uw.png" width="700px">

<br/>

### 2. Azure connection
#### Azure storage account
blob account 를 databricks 에 mount 하는 방법
Azure Blob Storage는 텍스트 또는 이진 데이터와 같이 많은 양의 구조화되지 않은 개체 데이터를 저장하는 서비스입니다. Blob Storage를 사용하여 세상에 공개적으로 표시하거나 애플리케이션 데이터를 비공개적으로 저장할 수 있습니다. Blob 스토리지의 일반적인 사용은 다음과 같습니다.    
https://docs.microsoft.com/ko-kr/azure/databricks/data/data-sources/azure/azure-storage

<br/>

#### Key vault, Synapse, Event Hubs, Cosmos DB
- key vault : azure 에서 제공하는 암복호화 서비스. SAS 토큰을 사용해서 secret 에 추가하고 연결...    
https://microsoft-bitools.blogspot.com/2020/02/use-azure-key-vault-for-azure-databricks.html
- Azure Event Hubs : 수백만 개의 이벤트를 수집, 변환 및 저장하는 하이퍼스케일(hyper-scale) 원격 분석 수집 서비스입니다. 분산형 스트리밍 플랫폼인 Azure Event Hubs는 대기 시간을 줄이고 구성 가능한 시간을 제공하므로 엄청난 양의 원격 측정을 클라우드에 입력할 수 있게 합니다. 또한 게시-구독 의미 체계를 사용하여 여러 애플리케이션에서 데이터를 읽을 수 있게 해줍니다.    
https://docs.microsoft.com/ko-kr/azure/databricks/scenarios/databricks-stream-from-eventhubs
- synapse : Azure Synapse Analytics(이전 SQL Data Warehouse)는 MPP(대규모 병렬 처리)를 활용하여 페타바이트 단위의 데이터에서 복잡한 쿼리를 빠르게 실행하는 클라우드 기반 Enterprise Data Warehouse입니다.    
https://docs.microsoft.com/ko-kr/azure/databricks/data/data-sources/azure/synapse-analytics
- cosmos : Azure Cosmos DB는 전 세계에 배포된 Microsoft의 멀티모델 데이터베이스입니다. Azure Cosmos DB를 사용하면 Azure의 여러 지리적 영역에서 처리량 및 스토리지를 탄력적이고 독립적으로 크기 조정할 수 있습니다. 포괄적인 SLA(서비스 수준 계약)를 통해 처리량, 대기 시간, 가용성 및 일관성을 보장합니다. Azure Cosmos DB는 여러 언어로 제공되는 SDK와 함께 다음 데이터 모델에 대한 API를 제공합니다.    
https://docs.microsoft.com/ko-kr/azure/databricks/scenarios/service-endpoint-cosmosdb


<br/><br/>
이러한 3-part training series 는 spark 를 사용하기 때문에 databrick에서의 spark 호출 방법을 배우는 것이 중요할 듯 싶다.    
azure storage account 를 제외하고는 특정 부서만 사용하지 않을까..

<br/>

#### Azure Data Factory

Azure Data Factory는 서버리스 스케일 아웃 데이터 통합 및 데이터 변환을 위한 Azure의 클라우드 ETL 서비스입니다. 코드가 필요 없는 UI로 직관적 작성 및 단일 창을 통한 모니터링 및 관리를 지원합니다. 기존 SSIS 패키지를 Azure로 리프트 앤 시프트하고 ADF에서 완전한 호환성을 사용하여 실행할 수도 있습니다. SSIS Integration Runtime은 완전 관리형 서비스를 제공하므로 인프라 관리에 대해 걱정할 필요가 없습니다.    

https://docs.microsoft.com/ko-kr/azure/data-factory/solution-template-databricks-notebook

<br/> 

#### CI/CD Azure DevOps

CI/CD(연속 통합 및 지속적인 업데이트)는 자동화 파이프라인을 사용하여 짧고 빈번한 주기로 소프트웨어를 개발하고 배달하는 프로세스를 말합니다. 코드의 빌드, 테스트 및 배포를 자동화함으로써 개발 팀은 많은 데이터 엔지니어링 및 데이터 과학 팀에서 여전히 널리 사용되는 수동 프로세스보다 더 자주 릴리스를 안정적으로 제공할 수 있습니다.

https://docs.microsoft.com/ko-kr/azure/databricks/dev-tools/ci-cd/ci-cd-azure-devops



# 2. Azure Databricks Security Fundamentals
Azure Databricks 에서는 통합된 데이터 서비스를 제공하고 있고, 대부분 보안인 Azure Active Directory 로 꽉 묶여있다.    
서로 다른 테넌트여도 한 region으로 통합한다.    

<br/><br/>

### 1. Network Security
Azure Databricks는 네트워크 보안을 보장한다. 공용 ip를 사용하기보다 자체 VNET (vritual network peering) 를 활용한다.    
Azure 에서 제공하는 네트워크 보안 방식은 다음과 같다.
1. 공용 ip 주소를 사용하지 않음 : 네트워크 연결을 위해 클러스터 노드에서 인바운트 포트를 열 필요가 없음. 클러스터 노드가 연결을 위해 public subnet이나 NAT gateway를 사용할 필요가 없음. control plane 과 data plane을 연결하기 위해 TLS 1.2를 사용하게 되면서 클러스터는 VNET의 아웃바운드만 연결하면 됨.

2. VNET injection : VNET Injection 없이 각 workspace akek VNET을 생성할 수도 있고, 이미 존재하는 VNET을 workspace 에 injection 할 수 있다. 그럼 서로 다른 workspace에서 VNET을 공유해서 배포할 수 있게된다. 

3. VNET Peering : VNET peering 은 두 개의 가상 네트워크를 연결하고, 개인 ip 주소를 이용해서 microsoft 백본 인프라를 통해 트래픽을 교환하는 메커니즘이다. Azure databricks 클러스터와 외부 데이터 소스 간의 보안 연결을 설정하는 데 사용할 수 있다.

4. IP Access Lists : ip access list 를 통해 관리자는 ip 주소들과 subnet 집합에 대해 UI alc API 액세스를 제한할 수 있다.

<br/><br/>

### 2. databricks에서 id 와 access 기능
id 제공은 single sing-on 방식이나 scim provisioning 방식이 있다.    
접근 권한은 ACL를 사용해서 Role-based access control 하거나 token management api를 활용해 제한한다.

<br/><br/>

### 3. Data protection
Azure databricks 는 data 보안을 보장한다. 방식은 다음과 같이 제공.
1. Data access control :
    -  Full user identity federation : 전체 id 에 대해서 data lock-in 을 방지하고 모든 시스템을 보안.
    - ADLS credential passthrough : Azure Databricks에 로그인하는 데 사용하는 것과 동일한 Azure AD(Active Directory) ID를 사용하여 Azure Databricks 클러스터에서 Azure Data Lake Storage Gen1(ADLS Gen1) 및 Azure Data Lake Storage Gen2(ADLS Gen2)에 자동으로 인증할 수 있습니다. 클러스터를 Azure Data Lake Storage 자격 증명 통과에 사용하도록 설정하면 스토리지에 액세스하기 위해 서비스 주체 자격 증명을 구성하지 않고도 해당 클러스터에서 실행하는 명령을 통해 Azure Data Lake Storage에서 데이터를 읽고 쓸 수 있습니다. https://docs.microsoft.com/ko-kr/azure/databricks/security/credential-passthrough/adls-passthrough
    - Table access control : 테이블 액세스 제어를 사용하면 Python 및 SQL의 데이터에 대한 액세스 권한을 프로그래밍 방식으로 부여하고 취소할 수 있습니다. 기본적으로 해당 클러스터에 대해 테이블 액세스 제어를 사용하도록 설정하지 않으면 모든 사용자가 클러스터의 관리 테이블에 저장된 모든 데이터에 액세스할 수 있습니다. 테이블 액세스 제어가 활성화되면 사용자는 해당 클러스터의 데이터 개체에 대한 사용 권한을 설정할 수 있습니다. https://docs.microsoft.com/ko-kr/azure/databricks/security/access-control/table-acls/


2. Encryption :
    - in-transit : 운송 중에 암호화하는 것. TLS 프로토콜 사용 (TLS는 SSL 보다 상위 보안)
    - at-rest : server-side encryption. azure 에서 제공해주는 key
    - secrets : Azure key vault. notebook 에 주로 사용. JDBC와 cloud 사이에서..


### 4. 규정
1. Security Standards : 국제 표준 보안
2. Cluster Policies : databricks 에서 설정 가능
3. Bring Your own Key : azure key vault 같은거
4. Diagnostic Logs : 특이 로그 모니터링

<br/><br/><br/>

# 3. Azure Databricks Identity Access Management
### 1. Admin Console
**databricks administration**    
databricks administration 에는 두 레벨이 있다. owner 와 administrators.    
owner : azure databricks 소유자, 기여자, 구독 관리자.    
administrators : user를 관리하고 그루핑. 접근 관리.

<br/>

**기능**
- manage user accounts : 유저생성, 읽기 권한 제공 등..
- manage groups
<br/>

**token-based authentication**    
Azure Databricks REST API에 접근하기 위해서는 token-based 의 인증을 받아야 한다.    
user setting 에서 새로운 토큰을 생성할 수 있다.
<br/>

**workspace storage**   
workspace storage 는 cascade 된 모든 storage, version history, cell, cluster log 등을 purge 하게 삭제할 수 있다.

<br/><br/>

### 2. Access Control
https://docs.databricks.com/security/access-control/index.html

<br/><br/><br/>

# 4. Azure Databricks Data Access Management
### 1. DBFS ( Databricks File System) and Hive Metastore Concepts
**DBFS**

DBFS 는 Azure databricks workspace 에 마운트되고, Azure databricks cluster에서 사용될 수 있는 분산파일 시스템이다. DBFS는 확장 가능한 객체 스토리지.
- 사용자 인증 정보 없이 데이터에 원활하게 액세스할 수 있도록 저장소 객체를 마운트할 수 있다.
- storage url 대신 디렉터리와 파일 의미 체계를 사용해 object storage 와 상호작용할 수 있다.
- 클러스터가 종료한 뒤에도 데이터 손실되지 않음.

<br/>
DBFS root    

- /Filestore : Save output files that you want to download to your local desktop.    
Upload CSVs and other data files from your local desktop to process on Databricks.
- /databricks-datasets : Apache spark 를 배우거나 알고리즘을 테스트하는 데 사용할 수 있는 공개 데이터 세트
- /databricks-results : 쿼리의 전체 결과를 다운로드하여 생성된 파일
- /databricks/init : 초기화 스크립트.
- /user/hive/warehouse : 외부가 아닌 hive table data. 
- /mnt : 외부 마운트된 스토리지


accessing DBFS
- notebooks : %fs, %sh 를 사용해서 command라인을 사용. databricks file system utilities 인 dbuilts.fs를 사용해서 업로드
- cli/api : dbfs cp -r test-dir dbfs:/test-dir    
dbfs cp test.txt dbfs:/test.txt
- file upload interface : 브라우저 통해서 업로드

<br/>

**Hive Metastore**    
The Hive Metastore stores metadata necessary to query relational entities, including schemas and locations. Metastore data is stored and managed in a relational database. This runs as a separate service, and is not tied to the lifetime of a cluster. 


<br/><br/>

### 2. Secure Access to Azure Data lake storage

#### Credential Passthrough

자격 증명 통과는 사용자 역할 기반 액세스 제어를 기반으로 provision된 파일 저장소에 대해 사용자 데ㅌ이터 액세스 범위를 제어한다. 클러스터 생성시 advanced options 에서 선택할 수 있다. 다음을 제한 할 수 있다.    

- Supports only Azure Data Lake Storage file systems.    
- Databricks REST API access.    
- Table access control: Azure Databricks does not suggest using credential passthrough with table access control. For more details on the limitations of combining these two features, see Limitations. For more information about using table access control, see Implement table access control.    
- Not suitable for long-running jobs or queries, because of the limited time-to-live on a user’s access token. For these types of workloads, we recommend that you use service principals to access your data.


여기서 !
How do you grant access to users or service accounts for more long-running or frequent workloads?    
원리가 어떻게 될까?    
When building a job in a notebook, you can add the following lines to the job cluster’s Spark configuration or run directly in the notebook.

```
spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", dbutils.secrets.get(scope = "<scope-name>", key = "<key-name-for-service-credential>"))
spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<directory-id>/oauth2/token")
```

<br/><br/>

### 3. Table Access Control

테이블 액세스 제어를 사용하면 Python 및 SQL의 데이터에 대한 액세스 권한을 프로그래밍 방식으로 부여하고 취소할 수 있습니다.

기본적으로 해당 클러스터에 대해 테이블 액세스 제어를 사용하도록 설정하지 않으면 모든 사용자가 클러스터의 관리 테이블에 저장된 모든 데이터에 액세스할 수 있습니다. 테이블 액세스 제어가 활성화되면 사용자는 해당 클러스터의 데이터 개체에 대한 사용 권한을 설정할 수 있습니다.

https://docs.microsoft.com/ko-kr/azure/databricks/security/access-control/table-acls/    
https://docs.microsoft.com/ko-kr/azure/databricks/security/access-control/table-acls/table-acl

<br/><br/>

### 4. Data isolation
workspace 를 레벨로 구분해서 부서별,팀별로 isolation 할 수 있고,    
한 클러스터 내에 멀티 유저가 공동으로 클러스터를 사용할 수도 있다.

<br/><br/><br/>

# 5. Azure Databricks Cluster Usage Management
### 1. computation Reseource

https://docs.microsoft.com/ko-kr/azure/databricks/clusters/

- Cluster
- Cluster Manager
- Job
<br><br/>
- Pool : 가상 머신을 가져오도록 클러스터를 선택적으로 구성할 수 있음. 클러스터 시작 및 자동 크기 조정 시간을 조절하고 유휴 상태를 조절 하고 바로 사용할 수 있는 인스턴스 집합.

### 2. Cluster configuration
- guardrails for hig concurrency clusters
- cost control with pools
- Databricks Runtime -> Apache spark 가 포함됨
- Autoscaling
- Auto Termination
- Instance Types : Databricks는 머신 유형을 적절한 workload type으로 구성.
- two tiers of VM instance : on-demand instance - 장기 약정없이 컴퓨팅 용량에 대해 초 단위 비용지불. spot instance - 여분의 vm 용량 사용하고 최대 가격을 선택.


<br/><br/><br/>

# 6. Azure Databricks SQL Administration

https://docs.microsoft.com/ko-kr/azure/databricks/scenarios/sql/    

https://docs.microsoft.com/en-us/azure/databricks/sql/api/    

single user 에서 권한을 설정할때 origin sql 과 동일하게 grant option을 사용한다.
