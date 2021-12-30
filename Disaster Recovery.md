# Diasaster Recovery — 

High Level Plan -  
Enterprise strategy is as follows:  
To plan for a like to like DR setup (AWS cloud infrastructure)  
at a different continent for ex: Ireland (Dublin) by a given timeline  

High Level Details -  
1. Mimic the production infrastructure & services
2. Required RPO - 1 hrs
3. Required RTO - 4 hrs

**DR Location**: Dublin, Ireland  
**DR review plan**: Annual  
DR will be supported by an internal AWS group team  



**`Cloud Artifacts Impacts`** —  
1. IAM Roles and Policies
2. S3 Buckets
3. Lambda 
4. Step Functions
5. CloudWatch
6. EMR (Transient and Persistent Clusters) - Persistent Cluster through Service Catalog
7. SQS
8. RDS Instance
9. Mongo DB Instances
10. SES
11. SNS
12. LEX
12. Cognito
13. LB for Datalake Portal
14. Kinesis Streams

**`Possible Challenges`** —  
Hardcoded values such as region-code s3-path, sqs name, etc.  
from the aws resources like lambda, step fn, configuration/environment files etc.  

|Component|DR change|Example|
|---|---|---|
|Hive DDL Storage Location|RDS Hive meta-store  replication to DR Environment|**Prerequisites** - \n 1. Prepare MySQL statements to update the S3 locations in hive meta-store in RDS, \n **Activity TBD on DR Testing Day** - 1. Replicate hive metastore to DR environment 2. Login and execute the prepared statements and validate|
|MongoDB Catalog Collection|MongoDB Disk Copy to DR environment|**Prerequisites** - \n 1. Prepare MongoDB Update queries to update following \n a.) Feed & Feed-Files Catalog i.) S3 Locations b.) Dedicated cluster configuration i.) SG, ii.) AMI, iii.) subnets iv.) log bucket namet etc.|
|cicd|DR specific property file creation|update all the key-values as per the DR environment|
|Step Func|remove hardcoded values||
|Lambda resources|remove hardcoded values||
|scala/java scr code files|remove hardcoded values||
|shell scripts|remove any hardcodes values||  


**`CICD Impacts`** —  
- New Jenkins file for Data Ingestion and Data Catalog specifically for DR
- HA Proxy setup for DR S3 DNS similar to prod –> prod.s3bucket.<org-name>.com 
- New environment file consisting all DR related details like :
  - S3_HOST_URL
  - Subnets
  - Security Groups
  - State Machine ARN
  - Role ARN
- All the artifacts except RDS, Mongo are generated programatically 
- S3 bucket names will change based on EU region standards   


**`AWS Resource List/Mappings/Counts`**:  
S3 buckets: 17  
Lambda: 41  
Step Fn: 4  
CloudWatch: 13  
EMR  
Ranger  
SQS: 32 including 50% DLQs  
RDS  
MongoDB (3 nodes/ips)  
SES  
SNS  
DR-Proxy  
DR-KMS-KEYS:  SQS, RDS, S3, EMR Local Disk Encryption  

  
**Reference:**  
1. https://www.youtube.com/watch?v=lK_t_dhUh5I
2. https://www.youtube.com/watch?v=cJZw5mrxryA
3. [disaster-recovery-whitepaper-on-aws.pdf](https://github.com/TheCodeCache/Cloud-AWS/files/7790861/disaster-recovery-whitepaper-on-aws.pdf)

