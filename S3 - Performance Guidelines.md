# Performance Guidelines for `S3` — 

When building applications that upload and retrieve objects from Amazon S3,  
we need to follow the best practices guidelines to optimize performance.  

- Measure Performance:  
When optimizing performance, look at network throughput, CPU, and DRAM requirements  
it's also worth looking at optimal EC2 instances  
It’s also helpful to look at DNS lookup time, latency, and data transfer speed,  
using HTTP analysis tools when measuring performance.  

- Scale Storage Connections Horizontally:
We can achieve the best performance by issuing multiple concurrent requests to Amazon S3  
- Use Byte-Range Fetches:
Using the `Range` HTTP header in a GET Object request, we can fetch a byte-range from an object,  
transferring only the specified portion.  
We can use concurrent connections to Amazon S3 to fetch different byte ranges from within the same object.  
- Retry Requests for Latency-Sensitive Applications:
Aggressive timeouts and retries help drive consistent latency.  
Given the large scale of Amazon S3,  
if the first request is slow, a retried request is likely to take a different path and quickly succeed.  
- Combine Amazon S3 (Storage) and Amazon EC2 (Compute) in the Same AWS Region:
Although S3 bucket names are globally unique, each bucket is stored in a Region that we select when we create the bucket.  
To optimize performance, we recommend that we access the bucket from Amazon EC2 instances in the same AWS Region when possible.  
This helps reduce network latency and data transfer costs.  
- Use Amazon S3 Transfer Acceleration to Minimize Latency Caused by Distance:
Amazon S3 `Transfer Acceleration` manages fast, easy, and secure transfers of files over long geographic distances between the client and an S3 bucket.  
`Transfer Acceleration` takes advantage of the globally distributed edge locations in `Amazon CloudFront`.  
As the data arrives at an edge location, it is routed to Amazon S3 over an optimized network path.  
`Transfer Acceleration` is ideal for transferring gigabytes to terabytes of data regularly across continents  
- Use the Latest Version of the AWS SDKs:
The AWS SDKs provide built-in support for many of the recommended guidelines for optimizing Amazon S3 performance.  



**Reference:**  
1. https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance-guidelines.html

