
# 1. Azure Databricks Cloud Architecture and System Intergration Fundamentals

### 1. Azure Databricks Reference Architecture
<img src="https://databricks.com/wp-content/uploads/2020/03/ETL-on-Azure_01.jpg" width="700px">
<img src ="https://miro.medium.com/max/1400/1*ij-beagXlQW-RRk3DyR_Uw.png" width="700px">

<br/>

### 2. Azure connection
### Azure storage account
blob account 를 databricks 에 mount 하는 방법
Azure Blob Storage는 텍스트 또는 이진 데이터와 같이 많은 양의 구조화되지 않은 개체 데이터를 저장하는 서비스입니다. Blob Storage를 사용하여 세상에 공개적으로 표시하거나 애플리케이션 데이터를 비공개적으로 저장할 수 있습니다. Blob 스토리지의 일반적인 사용은 다음과 같습니다.    
https://docs.microsoft.com/ko-kr/azure/databricks/data/data-sources/azure/azure-storage

<br/>

### Key vault, Synapse, Event Hubs, Cosmos DB
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

### Azure Data Factory

Azure Data Factory는 서버리스 스케일 아웃 데이터 통합 및 데이터 변환을 위한 Azure의 클라우드 ETL 서비스입니다. 코드가 필요 없는 UI로 직관적 작성 및 단일 창을 통한 모니터링 및 관리를 지원합니다. 기존 SSIS 패키지를 Azure로 리프트 앤 시프트하고 ADF에서 완전한 호환성을 사용하여 실행할 수도 있습니다. SSIS Integration Runtime은 완전 관리형 서비스를 제공하므로 인프라 관리에 대해 걱정할 필요가 없습니다.    

https://docs.microsoft.com/ko-kr/azure/data-factory/solution-template-databricks-notebook

<br/> 

### CI/CD Azure DevOps

CI/CD(연속 통합 및 지속적인 업데이트)는 자동화 파이프라인을 사용하여 짧고 빈번한 주기로 소프트웨어를 개발하고 배달하는 프로세스를 말합니다. 코드의 빌드, 테스트 및 배포를 자동화함으로써 개발 팀은 많은 데이터 엔지니어링 및 데이터 과학 팀에서 여전히 널리 사용되는 수동 프로세스보다 더 자주 릴리스를 안정적으로 제공할 수 있습니다.

https://docs.microsoft.com/ko-kr/azure/databricks/dev-tools/ci-cd/ci-cd-azure-devops
