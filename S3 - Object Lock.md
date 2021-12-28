# `S3 Object Lock` - 

With `S3 Object Lock`, we can store objects using a `write-once-read-many` (WORM) model.  
Object Lock can help prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely.  
We can use Object Lock to help meet regulatory requirements that require `WORM` storage,  
or to simply add another layer of protection against object changes and deletion.  

`Object Lock` provides two ways to manage object retention: 

1. Retention period —  
A `retention period` protects an object version for a fixed amount of time.
Specifies a fixed period of time during which an object remains locked.  
During this period, the object is WORM-protected and can't be overwritten or deleted.  
2. Legal hold —  
Provides the same protection as a retention period, but it has no expiration date.  
Instead, a `legal hold` remains in place until we explicitly remove it.  
Legal holds are independent from retention periods.  


`S3 Object Lock` provides two retention modes:  

1. Governance mode —  
users can't overwrite or delete an object version or alter its lock settings, unless they have special permissions  
3. Compliance mode —  
a protected object version can't be overwritten or deleted by any user, including the root user in our AWS account.

**Reference:**  
1. https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html

