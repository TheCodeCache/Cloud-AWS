# Monitoring Amazon S3 - 

We should create a monitoring plan as follows:  
- What are the monitoring goals?
- What resources to monitor?
- How often to monitor these resources?
- What monitoring tools will we use?
- Who will perform the monitoring tasks?
- Who should be notified when something goes wrong?

Monitoring Tools:  
- Amazon CloudWatch Alarms
- AWS CloudTrail Log Monitoring

Logging options:  
- Logging S3 API calls with CloudTrail
- Logging Server Access

Event Notifications:  
We can use the Amazon S3 Event Notifications feature to receive notifications when certain events happen in S3 bucket.  
S3 can publish notifications for the following events -  

- New object created events
- Object removal events
- Restore object events
- Reduced Redundancy Storage (RRS) object lost events
- Replication events
- S3 Lifecycle expiration events
- S3 Lifecycle transition events
- S3 Intelligent-Tiering automatic archival events
- Object tagging events
- Object ACL PUT events

S3 can send event notification messages to the following destinations.  
1. SNS
2. SQS
3. Lambda

**Imp. Note:**  
If the notification writes to the same bucket that triggers the notification, it could cause an execution loop.  
For example, if the bucket triggers a Lambda function each time an object is uploaded,  
and the function uploads an object to the bucket, then the function indirectly triggers itself.  
To avoid this, use two buckets, or configure the trigger to only apply to a prefix used for incoming objects.  

**Reference:**  
1. https://docs.aws.amazon.com/AmazonS3/latest/userguide/monitoring-overview.html

