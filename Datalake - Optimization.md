# Data-Lake Optimization - 

Optimizing a data lake environment includes minimizing operational costs  
We can reduce costs by optimizing how we use these services  

`Data asset storage` is often a significant portion of the costs associated with a data lake.  
Fortunately, AWS has several features that can be used to optimize and reduce costs.  
These include:  
`S3 Lifecycle management`,  
`S3 storage class analysis`,  
`S3 Intelligent-Tiering`,  
`S3 Storage Lens`,  
and Amazon `S3 Glacier` storage class.  

# Cost and Performance Optimization - 

- Using S3 best practices for data asset `naming` ensures high levels of performance.  
- A key best practice to reduce storage and analytics processing costs, and improve analytics querying performance,  
is to use an optimized data format, particularly a format like `Apache Parquet`.
- rightsizing of the S3 objects to `128 MB`, 
- partitioning based on business dates which are typically used while querying.  

