
# 0. What is the Databricks Lakehouse Platform?

### 1. What is Databricks?
Databricks는 Data, AI 회사로 알려져 있다. Databricks는 SaaS 를 제공하며, 빅데이터와 ai를 쉽게 매니징하고 모든 기업에서 데이터 혁신을 제공한다.    

- Ingest, process, and transform massive quantities and types of data
- Explore data through data science techniques, including but not limited to machine learning
- Guarantee that data available for business queries is reliable and up to date 
- Provide data engineers, data scientists, and data analysts the unique tools they need to do their work
- Overcome traditional challenges associated with data science and machine learning workflows (we will explore this in detail in our next lesson)

기존에는 big data 를 돌리기 위해서 많은 노력이 필요했다.     
Data warehousing(structured data -> data warehouse -> data markt -> analytics and bi)    
-> Data Engineering (data -> data lake (extract, transform, load..)     
    hadoop, apache spark, cloudera..)    
-> Streaming (apache kafka, apache spark)    
-> data science & Machine Learning....(jupyter..)     

등등등.. 아주 복잡했다. 서로 툴들이 다르니까..    

databricks overview : https://deepinsight.tistory.com/180

<br/><br/>

### 2. Datahouse, datalake, lakehouse...?

<img src="https://adatis.co.uk/wp-content/uploads/Lakehouse-Paradigm.jpg" width="700px"/>

데이터 웨어하우스는 정형화된 데이터를 사용자가 약간의 변화를 통해서 본다면, 데이터 레이크는 원시 데이터를 사용자가 원하는대로 가공하여 본다.    
먼저 data warehouse 와 data lake 차이를 알아보자 : https://loustler.io/data_eng/diff-data_lake-data_warehouse/    
<br/><br/>
lake house 는 ..    
데이터 웨어하우스를 분석 데이터베이스 위에 구축함으로써 기업이 사업 진행에 따라 클라우드를 기반으로 한 데이터 레이크를 확장 및 활용할 수 있게 해주는 것을 말한다.     
즉 기본적으로 데이터 웨어하우스를 클라우드 스토리지와 결합시키는 것을 의미한다고 간단하게 말할 수 있다.     
데이터 과학자들이 활용할 데이터가 양적인 측면에서 대폭 늘어날 뿐만 아니라, 데이터의 활용이 더 편리해진다는 뜻이 된다.

<br/><br/>
데이터 레이크의 한계를 해결하는 새로운 시스템 lake house가 등장하기 시작했습니다.    
레이크 하우스는 데이터 레이크와 데이터웨어 하우스의 최고의 요소를 결합한 새로운 개방형 아키텍처입니다.    
레이크 하우스는 새로운 오픈, 스탠다드 시스템 디자인으로 가능하게 되었습니다. 데이터 저장소에 사용되는 저비용 스토리지의 종류에 직접 데이터 웨어하우스와 유사한 데이터 구조 및 데이터 관리 기능을 구현합니다.    
     
이제 저렴하고 신뢰성이 높은 스토리지(객체 저장소의 형태로)를 사용할 수 있게 된 현대 사회에서 데이터 웨어하우스를 재설계해야 한다면 이러한 이점을 누릴 수 있습니다. 

<br/><br/>
데이터 레이크하우스란 무엇입니까?    
A data lakehouse is a new, open data management architecture that combines the flexibility, cost-efficiency, and scale of data lakes with the data management and ACID transactions of data warehouses, enabling business intelligence (BI) and machine learning (ML) on all data.    

데이터 레이크하우스: 단순함, 유연함 그리고 저렴한 비용    
데이터 레이크하우스는 새로운 오픈 시스템 디자인 입니다. 데이터 웨어하우스와 비슷한 데이터 구조와 데이터 관리 기능을 구현하되, 데이터 레이크에 쓰이는 저가 스토리지 상에 직접 구현하였습니다. 이 두가지를 하나로 병합함으로써 데이터 팀의 작업 속도가 빨라지는데, 이는 여러 시스템에 액세스하지 않고도 데이터를 사용할 수 있기 때문입니다. 또한 데이터 레이크하우스를 사용하면 팀원들이 가장 완전한 버전의 최신 데이터를 이용하여 데이터 사이언스, 머신 러닝과 비즈니스 분석 프로젝트를 수행할 수 있습니다.    

출처 : https://databricks.com/kr/glossary/data-lakehouse


<br/><br/><br/><br/>

# 1. What is Delta Lake?

### 1. Why Delta Lake?

<img src ="https://databricks.com/wp-content/uploads/2022/02/Marketecture.svg" width="700px">
<img src="https://images.squarespace-cdn.com/content/v1/5e336bbaa326860be67a714c/1604447655488-91PYKU9XE0D1B8F4L6UT/databricks-data-lakehouse-snowflake-cloud.png?format=1000w" width="700px" />

Delta Lake는 데이터 레이크 위에서 Lakehouse 아키텍처를 빌드할 수 있는 오픈 소스 프로젝트입니다.     
Delta Lake는 ACID 트랜잭션을 제공하고, 확장 가능한 메타데이터를 처리하고, 기존 데이터 레이크 위에서 스트리밍 및 일괄 처리 데이터 처리를 통합합니다.    




**정리하자면 delta lake는 data lakehouse 를 위한 오픈 data lake table 포맷.**

<br/><br/>

Delta Lake is an open format storage layer that puts standards in place in an organization’s data lake to provide data structure and governance. These standards ensure that all the data stored in a data lake is reliable and ready to use for business needs. It was developed at Databricks and open-sourced in early 2019.

Delta Lake was created by Databricks to help organizations address challenges with storing and managing big data using data lakes


Data Lakes : could handle all your data for data science and ML.
sotring all types of data (structred, unstructured, and semi-structured.
sotre data relatively cheaply compared to data warehouses.
however..... 
- poor BI support
- complex to set up
- poor performance
- unreliable data swamps
때문에 delta lake 를 만듦

<br/><br/>
Because they store all data types for long periods, data lakes can quickly become data swamps. Data swamps are data lakes that are difficult to navigate and manage. Delta Lake was designed to bring the governance and structure of data warehouses into data lakes to, above all else, ensure that the data in an organization’s data lake is reliable for use in big data and AI projects.


<br/><br/><br/>

### 2. How is Delta Lake used to Organize Data?
Delta lake 는 아키텍쳐 패턴에 따라서 데이터를 정리하여 successively cleaner table 로 만든다.    
bronze table -> siver tables -> gold table 순으로 data quality 를 올리는데,     
bronze 는 raw한 데이터 json, RDBMS, iot data 같은거고 gold 는 비지니스에 사용될만 한 버전의 데이터이다.
<br/><br/>

### 3. Elements of Delta Lake
<br/>

#### Delta files
<br/>
delta lake 는 customer의 데이터를 cloud storage 계정으로 저장한 Parquet file을 사용한다.    
Parquet is a columnar file format that provides optimizations to speed up queries and is a far more efficient file format than CSV or JSON. The columnar storage allows you to quickly skip over non-relevant data while executing queries.    
Delta files leverage all of the technical capabilities of Parquet files but have an additional layer over them. This additional layer tracks data versioning and metadata, stores transaction logs to keep track of changes made to a data table or object storage directory, and provides ACID transactions.

<br/>

#### Delta tables
<br/>
delta table registred in a Metastore    
A Delta table is a collection of data kept using the Delta Lake technology and consists of three things:    

- Delta files containing the data and kept in object storage

- A Delta table registered in a Metastore (a metastore is simply a catalog that tracks your data’s metadata - data about your data)

- The Delta Transaction Log saved with Delta files in object storage

<br/>

#### Delta Optimization Engine
<br/>
Delta Engine is a high-performance query engine that provides an efficient way to process data in data lakes.    
What the Delta Optimization Engine means for your business is that your data workloads run faster, so data times can perform their work in less time    

<br/>

#### Delta Lake storage layer
<br/>
When using Delta Lake, your organization stores its data in a Delta Lake Storage Layer and then accesses that data via Databricks. A key idea here is that an organization keeps all of this data in files in object storage. This is beneficial because it means your data is kept in a lower-cost and easily scalable environment.    

<br/>

간단히 말해 아래 기능을 제공한다.
- One platform for every use case, 
- High performance query engine
- structured transactional layer
- Data lake for all your data

<br/>
data lake 에는 너어어무 많은 문제가 있다. delta lake에서는 아래 방법으로 해결    

- ACID Transactions
- Schema management : Columns that are present in the table but not in the data are set to null. If there are extra columns in the data that are not present in the table, this operation throws an exception. This ensures that bad data that could corrupt your system is not written into it. Delta Lake also enables you to make changes to a table’s schema that can be applied automatically.
- scalable metadata handling : 분산 처리를 이용해서 큰 메타데이터도 일반 데이터처럼 처리함.
- unified batch and streaming data 
- Data versioning and time travel

<br/><br/>

정리해서 delta lake 의 장점은
- improve ETL pipeline
- unify batch and streaming (Apache spark 로 통합)
- BI on your data lake
- Meet regulatory needs (keep record of historical data change)

<br/><br/><br/>

# 2. What is Databricks SQL?
Databricks SQL 는 spark로 작동된다. 대부분의 경우에서 데이터를 접근하는 query는 크게 다르지 않다. SQL를 사용해서 데이터 쿼리와 분석을 계속할 수 있다. 그러나 쿼리를 시작할 때 관리자는 두 가지 세팅이 필요하다.    
1. A SQL Endpoint    
관리자가 sql endpoint를 endpoint메뉴에서 설정하는데, 특정 data object나 table, database, view에 사용자가 접근하는 것을 설정한다. 쿼리를 실행하기 위해서는 endpoint를 실행해주어야만 한다.
<br/>

2. Access to a Databricks Database.    
Query Editor 들어가서 SQL endpoint를 러닝 시키면 접근가능한 데이터베이스를 볼 수 있다. 데이터 베이스를 선택하면 스키마 브라우저에서 연동된 데이블을 볼 수 있다.

<br/><br/>

사용 방법
- endpoint 를 키면 접근할 수 있는 데이터베이스가 보이고, 메타데이터도 보임. 
쿼리를 사용해서 검색할 수 있고, 자동 완성 기능, 저장, 접근 제어 등등 할 수 있음
- 시각화 할 수 있음
- 공유할 수 잇음
- 대쉬보드 만들 수 있음
- 배치 프로그램을 만들 수도 있음
- 범위를 벗어나면 알람을 보낼 수도 있음

<br/><br/><br/>

# 3. What is Databricks Machine Learning?
lakehouse 가 뭐였지~?    
The Databricks Lakehouse platform provides data teams with the flexibility, cost-efficiency, and scale of data lakes with the data management and ACID transactions of data warehouses. This enables a variety of data practitioners – SQL analysts, data engineers, and machine learning practitioners – to perform their roles in a unified and scalable way.    
This Lakehouse platform is powered by data tools like Apache Spark, Delta Lake, and MLflow. These high-quality, open-source software are even more powerful when used together to help data teams build scalable, reliable, production-ready data pipelines.


<br/><br/>

### 1.  databricks lakehouse 에서 머신러닝 돌려야하는 이유?
- production machine learning depends on code and data : databricks ML platfrom 은 data-native 하기 때문에 데이터를 다른데로 옮길 필요가 없다.머신러닝 프로젝트를 databricks repo에 통합하기 쉽다.
- Machine learning requires many different roles to get involved : databricks platform 에는 data engineering 과 sql 분석들이 모두 있기 때문에 모든 팀들이 한 플랫폼에서 동일한 데이터를 취급한다. data scientist 와 machine learning engineer 가 같이 일할 수 있다.    
-> 클러스터 리소스가 부족해도 data scientist 가 같은 플랫폼에 있기에 scale up 할 수 있다.
- Machine learning requires integrating many different components.

- The Databricks Runtime for Machine Learning (Databricks Runtime ML) automates the creation of an environment optimized for machine learning.
One of the major advantages of the Databricks Runtime ML is the inclusion of the most popular machine learning libraries like TensorFlow, PyTorch, Keras, XGBoost, and Scikit-learn. These libraries have been chosen by Databricks as the tools data scientists and machine learning engineers can use throughout the machine learning lifecycle.
In addition to machine learning libraries, GPU-enabled ML runtimes are also available.

- Jobs can be created with specific environments, too, so that data teams are running their projects efficiently with all of the necessary libraries and dependencies.

- databricks repo를 사용해 프로젝트 저장 가능 : Using Databricks Repos, users can sync their existing projects or new projects from within the Databricks ML platform. By integrating with Git repositories like Github, Bitbucket, and GitLab, data scientists and machine learning engineers can follow development best practices and better follow common integration, deployment, and delivery processes.

<br/><br/>

### 2. MLflow
MLflow 는 전체 머신러닝의 라이프사이클을 관리하기 위해서 databricks에서 만든 오픈 소스이다.
다음 기능을 제공하고 있다.
- Tracking: Allows you to track your model runs within experiments to record and compare results.
두 가지 컨셉으로 저장됨 : Run, Expriments
- Models: Allow you to manage and deploy models from a variety of ML libraries to a variety of model serving and inference platforms.
MLflow은 머신러닝 모델로 기본 포맷을 제공한다. 가장 큰 장점은, 배포한 모델은 다른 작업에서도 동일한 코드와 작업으로 상호작용할 수 있다는 것이다. 서로 다른 모델에서 각각 전략을 설치할 필요가 없다는 것이다..

- Projects: Allow you to package ML code in a reusable, reproducible form to share with other data scientists or transfer to production.
- Model Registry: Allows you to centralize a model store for managing models’ full lifecycle stage transitions: from staging to production, with capabilities for versioning and annotating.
- Model Serving: Allows you to host MLflow Models for real-time serving.

<br/><br/>

### 3. AutoML

Databricks AutoML helps practitioners automatically apply machine learning to a dataset. It utilizes popular open-source machine learning libraries to build the models.
AutoML is available both through the Databricks ML user interface and as a Python API.
When using the AutoML API, users on Databricks ML will be able to automate an AutoML experiment using the same information provided in the UI workflow.

<br/><br/>

### 4. feature Store
Feature Store is a centralized repository and registry for features, or input variables, to machine learning models.
Feature Store provides a Feature Registry that keeps track of all features that have been created and facilitates reuse across different models. So if one data scientist develops a feature to use in a model, other data scientists will have access to that feature’s values and how that feature was created. This saves the organization time and money –

<br/><br/>

# 4. quiz

contact me : https://drive.google.com/file/d/1WR2efZy_i6IFJOIlgzwaFZyZt5DAjI_p/view?usp=sharing

------
------

