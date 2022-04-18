
# 0. Overview
SQL 과 python 을 활용해서 Lakehouse 의 분석 애플리케이션, 대시보드 활용 등을 학습.
- Databrcicks SQL
- Delta Live Tables
- Databricks Repos
- Databricks Task Orchestration,
- Unity Catalog

<br/><br/>

# 1. Access coures materials

- workspace  에서 동작하는 data engineering 특징
- sql 문 사용해서 데이터베이스를 가져오고 읽는 법
- vacuuming 방법 : 데이터를 지우기 전에 optimizing 하는 방법

https://docs.microsoft.com/ko-kr/azure/databricks/delta/

https://docs.delta.io/latest/delta-batch.html#

vacuumming :https://docs.microsoft.com/en-us/azure/databricks/spark/latest/spark-sql/language-manual/delta-vacuum

<br/><br/>


# 2. ELT with spark SQL and python
### 2.1 Relational Entities on Databricks
- Databases and Tables on Databricks : database 를 가져오고, 테이블 읽고, 삭제하고, 지우는데 cascade
- views and CTE on Databricks : Common table expression (CTE)    
use spark SQL DDL, run queies that use common table expressions, create view

<br/>


### 2.2 ETL with spark SQL
- Extracting Data Directly from files 
- Providing Options for External Sources : delimiter 를 option 에 추가하여 text, cvs 등등의 inline 데이터를 테이블로 변경    
jdbc를 use 와 option에 사용해서 사용 가능
- creating Delta Table : CTAS ( create table _ as select) statements.    
CTAS statemnets do not suppoert schema declaration.    
contraint 정책이 있음. 만약 write 실패될 때 contraint 를 살펴볼 것. NOT NULL, CHECK     
partitioned by, shallow clone
- writing to delta tables : insert overwrite, insert into, merge into, copy into
- Extract and Load Data : JSON to delta table
- Cleaning Data : distinct, group by
- Advanced SQL Transformations : using, working with json, flatting and unpacking, combining dataset using joins and set operations, reshaping data using pivot tables, using higher order functions for working with arrays.     
: syntax, collect_set, flatten, array_distinct, union, intersect, minus, transactions, filter, exist, transform, reduce, filter, transform

<br/>

### 2.3 SQL UDFs and Control Flow
- SQL UDF : 사용자 정의 함수
- CASE/WHEN 

<br/>

### 2.4 Reshaping Data 
- create clickpaths table : 각 사용자가 이벤트에서 특정 작업을 수행한 횟수를 집계한 다음 이전 노트북에서 만든 flattened view transaction 과 결합한 표.
- sales : item name 에서 정보 추출.

<br/><br/>

# 3. Just Enough Python for spark SQL
위에 연습했던 sql을 python에서 spark 를 만으로 동작 가능.

<br/><br/>

# 4. Incremental Data Processing with Structured Streaming and Auto Loader

https://docs.microsoft.com/ko-kr/azure/databricks/spark/latest/structured-streaming/auto-loader

자동 로더는 새로운 데이터 파일이 클라우드 스토리지에 도착하면 점진적이고 효율적으로 처리합니다. 자동 로더는 한 시간에 수백만 개의 파일이 로드되는 파이프라인으로 백필해야 하는 수십억 개의 파일이 포함된 스토리지 계정의 데이터 로드로 확장할 수 있습니다.

argument : data_source, source_format, table_name, checkpoint_directory

- Reasoning about Incremental Data : 방대한 양의 inifite Data stream 이 들어오면 unbouned table로 변환한다.    
new data in the data stream = new rows appended to a unbounded table    
real time system 제공가능   
end-to-end fault tolerance : 실패 복구, 실패 재실행 등을 보장.    
batch 같은 작업은 가능하지만 특정 작업은 제공하지 않는다. (sort 같은거..)    
persisting streaming results    
tringger interval    
pulling it all together
spark SQL 언어로도 가능     

<br/>

### 4.1 The Medallion Architecture in the Data lakehouse
#### Multi-Hop Architecture

메달리온 아키텍처 는 데이터가 아키텍처 의 각 레이어를 통과할 때 데이터의 구조와 품질을 점진적으로 개선하는 것을 목표로 레이크하우스에서 데이터를 논리적으로 구성하는 데 사용되는 데이터 디자인 패턴입니다(브론즈 ⇒ 실버 ⇒ 골드 레이어 테이블 ) . . 메달리온 아키텍처는 때때로 "멀티 홉" 아키텍처라고도 합니다.    


회기 : 레이크하우스 아키텍처의 이점    

- 단순 데이터 모델
- 이해하기 쉽고 구현하기
- 증분 ETL 활성화
- 언제든지 원시 데이터에서 테이블을 다시 생성할 수 있습니다.
- ACID 트랜잭션, 시간 여행

Bronze Layer
- typically just a raw copy of ingested data
- replaces traditional data lake
- provides efficient storage and querying of full, unprocessed history of data

Silver Layer
- Reduces data storage complexity, latency, and redundancy optimizeds ETL throughput and analytic query performance perserves grain of original data (without aggregations)
- Eliminates duplicate records
- Production schema enforced
- Data quality checks, corrupt data quarantined

Gold Layer
- powers ML applicications, reporting, dashboards, ad noc analytics
- Refineds views of data, typically with aggregations
- Reduces strain on production systems.
- Optimizes query performance for business-critical data


raw data 에서 bronze -> silver -> gold 데이터로 변경하고, 그래프 생성하기

<br/>

#### Delta Live Tables

https://databricks.com/kr/product/delta-live-tables

Make reliable ETL easy on Delta Lake
- Operate with agility : Delclarative tools to build batch adn streaming data pipeline
- Trust your data : DLT has built-in declarative quality controls.    
Declare quality expectations and actions to take
- Scale with reliabiltiy : Easily scale infrastructure alongside your data


Delta live table 은 ui page, SQL (create incremental live table) 을 통해 이용 가능하다.


<br/><br/>

# 5. Managing Data Access and Production Pipelines
#### 5.1 Task Orchestration with Databricks jobs
DLT (Delta live table) 와 multi job을 돌리는 노트북 그 결과를 돌리는 다른 노트북들을 연결하는 파이프라인을 UI를 통해 만들 수 있다.

<br/>

#### 5.2 Running your frist DBSQL Query - Navigating Databricks SQL and Attaching to Endpoingts
workspace SQL 에 해당하는 작업.    

<br/>

#### 5.3 Managing permissions in the Lakahouse
Unity catalog : acl 이나 table access 등 불편한 permission 제공 방법을 개선한 방법.

<br/>

#### 5.4 Productionalizing Dashborads and Queries in DBSQL
Dashborad 에서 할 수 있는 것들~
1. Create a Query to Track Totle Records with batch program.
2. Create a Bar graph visualization
3. Create a New Dashboard
4. Create a Query to Calculate the Recent Average Ping
5. Add a line plot visualization to your Dashboard
6. create a qery to report summary statistics
7. Add the summary table to your Dashboard
8. Review and Refresh your Dashboard
9. Share your Dashboard
10. Set up an alert
11. Review alert Destination options

DLT pipeline 에서 할 수 있는 것들~ ( End to End ETL in the Lakehouse )
1. Create and Configure a DLT pipeline
2. Schedule a Notebook job
3. Set a Chronological schedule for your job
4. Register DLT event metrics for Querying with DBSQL
5. Define a Query on the Gold table (at new query page)
6. Add a line plot visualization
7. Track Data processing progress (at new query page)
8. Refresh your Dashboard and track results
9. Execute a Query to Repari Broken Data
10. Consider Production data permissions
11. Shut Down production infrastructure

