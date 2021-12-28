# Data-Lake Monitoring - 

A key aspect of operating a data lake environment is understanding how all of the components,  
that comprise the data lake are operating and performing,  
and generating notifications when issues occur or operational performance falls below predefined thresholds.  

**Amazon CloudWatch** -  

`CloudWatch` is a monitoring service for AWS Cloud resources and the applications that run on AWS.  

We can use CloudWatch to 
- collect and track metrics, 
- collect and monitor log files, 
- set thresholds, and, 
- initiate alarms. 

This allows us to automatically react to changes in your data-lake built on `S3`.  

We can —   
- use CloudWatch metrics to understand and improve the performance of applications that are using `Amazon S3`  
- We can also monitor the requests to the data-lake built on `S3` and identify and act upon operational issues quickly  
- We can also monitor the S3 APIs that are pending replication, total size,  
and the maximum time required for replication to the destination Region  



**Amazon CloudTrail** —  

An operational data lake has many users and multiple administrators,  
and may be subject to compliance and audit requirements,  
so it’s important to have a complete audit trail of actions taken and who has performed these actions.  
`AWS CloudTrail` is an AWS service that enables  
`governance`, `compliance`, `operational auditing`, and `risk auditing` of AWS accounts.  

CloudTrail continuously monitors and retains events related to API calls across AWS services that comprise a data lake  
CloudTrail provides a history of AWS API calls for an account,  
including API calls made through the AWS Management Console, AWS SDKs, command line tools,  
and most data lakes built on S3 services  

We can identify which users and accounts made requests or took actions against AWS services,  
that support CloudTrail, the source IP address the actions were made from, and when the actions occurred.  

CloudTrail can be used to simplify data lake compliance audits by automatically recording and storing activity logs for actions made within AWS accounts.

Integration with Amazon CloudWatch logs provides a convenient way to  
search through log data, identify out-of-compliance events,  
accelerate incident investigations,  
and expedite responses to auditor requests.  
CloudTrail logs are stored in a separate logs bucket within your data lake for durability and deeper analysis.  

**Reference:**  
1. https://docs.aws.amazon.com/whitepapers/latest/building-data-lakes/monitoring-optimizing-data-lake-environment.html

