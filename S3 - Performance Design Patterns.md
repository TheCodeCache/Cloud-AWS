# Performance Design Patterns for `Amazon S3` — 

To optimize performance, we can use the following design patterns:  

1. Using Caching for Frequently Accessed Content:  
If a workload is sending repeated GET requests for a common set of objects,  
we can use a cache such as `CloudFront`, `ElastiCache` to optimize performance.  
Applications that use caching also send fewer direct requests to Amazon S3, which can help reduce request costs.  

Amazon CloudFront:  
It is a fast content delivery network (CDN) that transparently caches data from Amazon S3,  
in a large set of geographically distributed points of presence (PoPs).  
When objects might be accessed from multiple Regions, or over the internet,  
CloudFront allows data to be cached close to the users that are accessing the objects.  
This can result in high performance delivery of popular Amazon S3 content  

Amazon ElastiCache:  
It is a managed, in-memory cache.  
With ElastiCache, we can provision Amazon EC2 instances that cache objects in memory.  
This caching results in orders of magnitude reduction in GET latency and substantial increases in download throughput.  
To use ElastiCache, we modify application logic to both populate the cache with hot objects,  
and check the cache for hot objects before requesting them from Amazon S3  

2. Timeouts and Retries for Latency-Sensitive Applications:  
If an application generates high request rates  
(typically sustained rates of over 5,000 requests per second to a small number of objects),  
it might receive `HTTP 503 slowdown responses`.  
If these errors occur, each AWS SDK implements automatic retry logic using `exponential backoff`.  
If we are not using an AWS SDK, we should implement retry logic when receiving the HTTP 503 error  

For latency-sensitive applications, Amazon S3 advises tracking and aggressively retrying slower operations.  
When we retry a request, it's recommended to use a new connection to Amazon S3 and performing a fresh DNS lookup.  

If we are using AWS Key Management Service (AWS KMS) for server-side encryption, then need to check the Limits in the AWS KMS    

3. Horizontal Scaling and Request Parallelization for High Throughput:  
For high-throughput transfers, Amazon S3 advises using applications that use multiple connections to GET or PUT data in parallel.  
For example, this is supported by Amazon S3 Transfer Manager in the AWS Java SDK,  
and most of the other AWS SDKs provide similar constructs.  
For some applications, we can achieve parallel connections by launching multiple requests concurrently in different application threads,  
or in different application instances.  
The best approach to take depends on the application and the structure of the objects that we are accessing.  

As a general rule, when we download large objects within a Region from Amazon S3 to Amazon EC2,  
AWS suggest making concurrent requests for byte ranges of an object at the granularity of 8–16 MB.  
Make one concurrent request for each 85–90 MB/s of desired network throughput.  
To saturate a 10 Gb/s network interface card (NIC), we might use about 15 concurrent requests over separate connections.  
We can scale up the concurrent requests over more connections to saturate faster NICs, such as 25 Gb/s or 100 Gb/s NICs.  

**Very Important**  — 
**Measuring Performance**:  
It is important when we tune the number of requests to issue concurrently.  
AWS recommends starting with a single request at a time.  
Measure the network bandwidth being achieved and the use of other resources that the application uses in processing the data.  
We can then identify the bottleneck resource (that is, the resource with the highest usage),  
and hence the number of requests that are likely to be useful.  
**For example**, if processing one request at a time leads to a CPU usage of 25 percent,  
it suggests that up to four concurrent requests can be accommodated.  
Measurement is essential, and it is worth confirming resource use as the request rate is increased.  

If the application issues requests directly to Amazon S3 using the REST API,  
AWS recommends using a pool of HTTP connections and re-using each connection for a series of requests  

It’s worth paying attention to DNS and double-checking that requests are being spread over a wide pool of Amazon S3 IP addresses.  
Network utility tools such as the netstat command line tool can show the IP addresses being used for communication with Amazon S3  

4. Using Amazon S3 Transfer Acceleration to Accelerate Geographically Disparate Data Transfers:  
Amazon S3 `Transfer Acceleration` is effective at minimizing or eliminating the latency caused by,  
geographic distance between globally dispersed clients and a regional application using Amazon S3.  
Transfer Acceleration uses the globally distributed edge locations in CloudFront for data transport.  
The AWS edge network has points of presence in more than 50 locations  

**Reference:**  
1. https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance-design-patterns.html

