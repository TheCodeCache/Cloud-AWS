# How to reduce S3 costs ? 

Let's review the factors that affect Amazon S3 monthly costs. We will pay in terms of:  

- The size of data stored each month (GB).
- The number of access operations completed (e.g. PUT, COPY, POST, LIST, GET, SELECT, or other request types).
- Number of transitions between different classes.
- Data retrieval size and amount of requests.
- Data transfer fees (bandwidth out from Amazon S3)

`One of the most important cost factors is the storage class`  

We need understand `Storage Class`:  
there are 6 of them as follows -  
1. S3 Standard  (used for frequently accessed data)
2. S3 Intelliegent-Tiering  
   (it automatially moves the data b/w `Standard` to `Standard IA` when it detects an object is not frequently getting accessed)  
3. S3 Standard IA (Infrequent Access)
4. S3 One Zone IA
5. S3 Glacier (data access cost is high, ideal for backs/archive)
6. S3 Glacier Deep Archive (data access cost is high, ideal for backs/archive)

note:  We can assign different classes to an object belonging to same bucket.  

Recommendation based on Access Frquency:  
- **Access Frequency**          **Recommended S3 Class**
- Every 30 (or less) days         S3 Standard
- B/w 30 to 90 days          S3 Intelligent-Tiering
- B/w 90 to 180 days             S3 Glacier
- Every 180 (or more) days       S3 Glacier Deep Archive  

The main strategies to reduce AWS S3 costs are as follows:  

1. Analyze the access patterns for your data and set the right S3 class at the object creation time.
2. Adjust the S3 class for existing objects
since, the # of objects could be huge so it becomes difficult to find the access patterns, hence AWS provides a tool at small cost:  
`S3 Storage Class Analysis` is a tool that will help us with this activity to find access patterns of existing objects  
3. Remove unused S3 objects  
4. `Use S3 Lifecycle Management` - **the best options of all**
5. Expire S3 objects (objects will be removed permanently when it expires)
6. Expire incomplete multipart uploads
7. Compress S3 objects (while uploading)
8. Pack S3 objects (while downloading)
9. Limit object versions
10. Use Bulk retrieval mode for S3 Glacier

11. Use `Query in Place` functionality - **important concept**
Some applications store tables as Amazon S3 objects. These tables have a specific format (like JSON, CSV, or Apache Parquet formats).  
To query the data, you have to download the whole file. And then you have to query the whole table to find the desired data inside the table.  

But there are more efficient ways to get the contents. AWS offers tools like Amazon Athena, S3 Select, or Amazon Redshift Spectrum.  
These tools allow you to perform queries directly on the cloud. They process the data using SQL commands. And then they send you the data you need.  

Advantages:  
a. less processing power locally  
b. download less data from S3 (thus saves money on bandwidth)  
c. this makes the process faster and cheaper  
Note -  that S3 queries have a small additional cost  

12. Change Region - 
Some regions are much expensive than others. And this applies to Amazon S3 prices also. So it’s worth considering moving your S3 bucket to a region with lower prices.  

Another factor to consider is data transfer costs between AWS regions. Data sent from a bucket to a VPC in the same region is free.  
But sending data to a VPC in another region will have a cost per Gb. It’s a good idea to keep a bucket in the region where data is sent.  

We need to `choose region` based on below 4 points:
a. latency -   
  Latency is the time it takes for the data to travel from the user to your servers, and return.  
  Interactive applications, where the user sends data and expect a quick response, are sensible to latency.  
b. costs -  
  The price of each AWS Service differs according to the region. For example, services in US regions are generally the least expensive  
c. legal considerations -  
  companies might have restrictions regarding where to store the information.  
d. serice availability -  
  All AWS regions have core services (for example AWS EC2, S3, and RDS). But newer AWS services aren’t available in all regions  

**Reference:**  
1. https://www.iobasis.com/Strategies-to-reduce-Amazon-S3-Costs/


