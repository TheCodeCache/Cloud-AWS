# S3 - Data Consistency Model - 

Amazon S3 provides strong `read-after-write` consistency for `PUT` and `DELETE` requests of objects  
in the Amazon S3 bucket in all AWS Regions.  
This behavior applies to both  
- `writes` to new objects  
- `PUT` requests that `overwrite` existing objects and `DELETE` requests  

In addition,  
- read operations on Amazon S3 Select,  
- Amazon S3 access controls lists (ACLs),  
- Amazon S3 Object Tags,  
- and object metadata (for example, the HEAD object) 
are also strongly consistent.  

Updates to a single key are atomic, that means it never returns partial or corrupt data.  

Amazon S3 achieves high availability by replicating data across multiple servers within AWS data centers.  
If a PUT request is successful, the data is safely stored.  
Any read (GET or LIST request) following the receipt of a successful PUT response will return the data written by the PUT request

1. A process writes a new object to Amazon S3 and immediately lists keys within its bucket.  
The new object appears in the list.
2. A process replaces an existing object and immediately tries to read it.  
Amazon S3 returns the new data.
3. A process deletes an existing object and immediately tries to read it.  
Amazon S3 does not return any data because the object has been deleted.
4. A process deletes an existing object and immediately lists keys within its bucket.  
The object does not appear in the listing.  

**Note:**  
1. Amazon S3 does not support object locking for concurrent writers.  
If two PUT requests are simultaneously made to the same key,  
the request with the latest timestamp wins.  
If this is an issue, we must build an object-locking mechanism into the application.  
2. Updates are key-based. There is no way to make atomic updates across keys.  
For example, WE cannot make the update of one key dependent on the update of another key,  
unless we design this functionality into the application  

`Bucket` configurations have an eventual consistency model -  
1. If we delete a bucket and immediately list all buckets,  
the deleted bucket might still appear in the list.
2. If we enable versioning on a bucket for the first time,  
it might take a short amount of time for the change to be fully propagated.  
it's recommended that we should wait for 15 minutes after enabling versioning,  
before issuing write operations (PUT or DELETE requests) on objects in the bucket.

# Concurrent applications - 

Let us analyze when multiple clients are writing to the same items.  
 
**Case-1:** both W1 (write 1) and W2 (write 2) finish before the start of R1 (read 1) and R2 (read 2).  
Because, S3 is strongly consistent, R1 and R2 both return color = ruby.  
 ![image](https://user-images.githubusercontent.com/26399543/147595885-78c82108-4e7a-47f5-b6da-0f9567ad3716.png)  

**Case-2:** W2 does not finish before the start of R1. Therefore, R1 might return color = ruby or color = garnet.  
However, because W1 and W2 finish before the start of R2, R2 returns color = garnet.  
![image](https://user-images.githubusercontent.com/26399543/147595966-e4e35f06-17eb-410f-b80a-4e40930dc6f9.png)  

**Case-3:**  Concurrent Writes, i.e. when W2 begins before W1 has received an acknowledgement.  
![image](https://user-images.githubusercontent.com/26399543/147596219-33b07605-a26a-42c9-b8a1-2ef0b261f584.png)  

In the last case of `concurrent writes`, it works as follows:  
Amazon S3 internally uses last-writer-wins semantics to determine which write takes precedence.  
However, the order in which Amazon S3 receives the requests and the order in which applications receive acknowledgements,  
cannot be predicted because of various factors, such as network latency.  
For example, W2 might be initiated by an Amazon EC2 instance in the same Region,  
while W1 might be initiated by a host that is farther away.  
The best way to determine the final value is to perform a read after both writes have been acknowledged.  


**Reference:**  
1. https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html#ConsistencyModel


