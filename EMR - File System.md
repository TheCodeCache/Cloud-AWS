# EMRFS - EMR FileSystem 

The EMR File System (EMRFS) is an implementation of HDFS  
that all Amazon EMR clusters use for reading and writing regular files from Amazon EMR directly to Amazon S3.  
EMRFS provides the convenience of storing persistent data in Amazon S3 for use with Hadoop  
while also providing features like data encryption.  

For security purpose - We can use different IAM roles for EMRFS requests to Amazon S3  
based on cluster users, groups, or the location of EMRFS data in Amazon S3.  

**`WARNING`** - 
Before turning on `speculative execution` for Amazon EMR clusters running Apache Spark jobs, review below details:

EMRFS includes the EMRFS S3-optimized committer, an OutputCommitter implementation  
that is optimized for writing files to Amazon S3 when using EMRFS.  
If you turn on the `Apache Spark speculative execution` feature with applications  
that write data to Amazon S3 and do not use the EMRFS S3-optimized committer,  
you may encounter data correctness issues described in this Jira - SPARK-10063  

This can occur if you are writing files to Amazon S3 using formats such as ORC and CSV,  
which are not supported by the EMRFS S3-optimized committer.  

`EMRFS direct write` is typically used when the EMRFS S3-optimized committer is not supported,  
such as when writing the following:  

- An output format other than Parquet, such as ORC or text.
- Hadoop files using the Spark RDD API.
- Parquet using Hive SerDe

`EMRFS direct write` is not used in the following scenarios:  
- When the EMRFS S3-optimized committer is enabled
- When writing dynamic partitions with partitionOverwriteMode set to dynamic.
- When writing to custom partition locations, such as locations  
that do not conform to the Hive default partition location convention.  
- When using file systems other than EMRFS, such as writing to HDFS or using the S3A file system.  

o determine whether your application uses direct write in Amazon EMR,  
enable Spark INFO logging. If a log line containing the text  
"Direct Write: ENABLED" is present in either Spark driver logs or Spark executor container logs,  
then your Spark application wrote using direct write.  

By default, speculative execution is turned OFF on Amazon EMRclusters.  
AWS highly recommend that you do not turn speculative execution on if both of these conditons are true:  

- You are writing data to Amazon S3.
- Data is written in a format other than Apache Parquet 
- or in Apache Parquet format not using the EMRFS S3-optimized committer.

If you turn on Spark speculative execution and write data to Amazon S3 using EMRFS direct write,  
you may experience intermittent data loss. When you write data to HDFS,  
or write data in Parquet using the EMRFS S3-optimized committer,  
Amazon EMR does not use direct write and this issue does not occur.  

**`Recommendation to work with Speculative Execution`** -  
If you need to write data in formats that use EMRFS direct write from Spark to Amazon S3 and use speculative execution,  
we recommend writing to HDFS and then transferring output files to Amazon S3 using S3DistCP.  


**Reference:**  
1. https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-fs.html


