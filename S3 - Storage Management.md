# Storage Management - 

Amazon S3 has `storage management` features that we can use - 
- to manage costs,  
- meet regulatory requirements,  
- reduce latency,  
- save multiple distinct copies of data  
for compliance requirements.  

**S3 Lifecycle** – 
Configure a lifecycle policy to manage the objects and store them cost effectively throughout their lifecycle.  
We can transition objects to other S3 storage classes or expire objects that reach the end of their lifetimes.

**S3 Object Lock** – 
Prevent Amazon S3 objects from being deleted or overwritten for a fixed amount of time or indefinitely.  
We can use Object Lock to help meet regulatory requirements that require write-once-read-many (WORM) storage,  
or to simply add another layer of protection against object changes and deletions.  

**S3 Replication** – 
Replicate objects and their respective metadata and object tags,  
to one or more destination buckets in the same or different AWS Regions for reduced latency, compliance, security, and other use cases.  

**S3 Batch Operations** – 
Manage billions of objects at scale with a single S3 API request or a few clicks in the Amazon S3 console.  
We can use Batch Operations to perform operations,  
such as Copy, Invoke AWS Lambda function, and Restore on millions or billions of objects.  

**Reference:**  
1. https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html

