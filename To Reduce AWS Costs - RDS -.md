# To reduce AWS Costs - RDS — 

This Amazon RDS cost-optimization approach consists of the following key steps:  
![image](https://user-images.githubusercontent.com/26399543/147840980-1518f139-7502-4fdd-ac5b-9ae338ddbfb1.png)

if you're using a read replica of type db.r4.4xlarge (16 vCPUs) that is 30% utilized,  
you should consider downsizing to db.r4.2xlarge (8 vCPUs).  
Now that you have half the number of vCPUs, your CPU utilization is expected to increase to around 60–70%.  
This leaves a buffer of around 30% for unexpected spikes and future organic increases in traffic.  

Strategy to reduce RDS costs:  

1. Changing the DB engine  

![image](https://user-images.githubusercontent.com/26399543/147840812-43c76af1-4f70-4953-ac3a-eb31da88e806.png)

2. Using Amazon EC2 instead of Amazon RDS (Trade-Off)  

![image](https://user-images.githubusercontent.com/26399543/147840817-173237ce-8406-4240-a54f-50cc4223e131.png)

 Using EC2:  
  upside - it's cost effective, more flexibility regarding the types of EC2 instances you can choose   
   For example, there are 254 types of EC2 instances and only 18 RDS database instance types.  
  downside - overhead of deploying and maintaining the database yourself.  

 Using RDS:  
  upside - lots of benefits, makes the database administration much easier  
  downside - it brings higher costs  

3. Right-Sizing your database instance  
The right approach is to determine your database requirements (including memory, CPU, IOPs, and others).  
And then choosing the most cost-effective instance that complies with them  
4. Using Reserved DB Instances  
upside - Purchasing Reserved Instances will allow you to save 30% to 64%  
downside - need to commit to AWS to use the database instance for 1 or 3 years.
5. Changing the database Region  
6. Right-Sizing Database Storage  
Amazon RDS supports 3 types of storages:
- General Purpose (SSD) Storage
- Provisioned IOPS (SSD) Storage (recommended for I/O-intensive workloads)  
- Magnetic Storage  
The number of input/output operations per second that the storage can perform is called `IOPS`.  
General Purpose SSD storage assigns a baseline IOPS performance.  
And it also has bursting capacity.  
That means that the storage can respond with higher IOPS than the baseline, but only for short periods.  

We can check current storage usage with Amazon Cloudwatch metrics  
(for example ReadIOPS, WriteIOPS, ReadLatency, and WriteLatency).  

7. Using Storage AutoScaling  
Sometimes the future capacity isn't well known, and storage could be over-provisioned.  
But remember that you pay for each GB provisioned, even if it's not used. So this over-provisioning will cost you money.  
This storage autoscaling feature automatically expands the RDS storage size when free space is low.  

8. Reducing back-up Retention Period 


Reference:  
1. https://aws.amazon.com/blogs/database/optimizing-costs-in-amazon-rds/
2. https://www.iobasis.com/Strategies-to-reduce-Amazon-RDS-Costs/

