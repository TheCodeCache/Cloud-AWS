**How its different from AWS SQS ?**  
SQS is a fully managed message queuing service,  
that enables you to decouple and scale microservices, distributed systems, and serverless applications.  
Using SQS, you can send, store, and receive messages between software components at any volume, without losing messages.  
By Nature its not meant for real time ingestion but though it can handle msgs we are using it.  
Comparing SQS with Kinesis is like comapring Apple to Orange. 
But at Operational front currently `we need to poll the msgs from SQS using external service of AWS Lambda`,  
where in `Kinesis firehose will give functionality to put the msgs directly to other target system without any other service`.  
SQS can't deliver the data directly to any services like Redshift, S3 or ElasticSearch.  
Kinesis analytics can't take input from SQS.  


**Benefits of Migrating from SQS to Kinesis Firehose**  
Kinesis can bring in easy interfacing with Kinesis Analytics, Amazon S3, Amazon Redshift, Amazon Elasticsearch Service.
We can also consume msg directly on EMR from Firehose and achive real time ingestion with spark streaming.
On the fly creation of orc file dumps, transofrmations through Lambda of raw msgs

