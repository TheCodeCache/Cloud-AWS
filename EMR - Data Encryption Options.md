# Encryption Options —  

![image](https://user-images.githubusercontent.com/26399543/147822958-96a65563-8bff-43e4-a8c3-fd725b56c14b.png)


With Amazon EMR, there're 2 options as follows:  

1. you can use a `security configuration` to specify settings for encrypting data at rest, data in transit, or both.  
2. When you enable at-rest data encryption, you can choose to encrypt EMRFS data in Amazon S3, data in local disks, or both  

Amazon EMR `security configurations` to configure data encryption settings for clusters more easily.  
`Security configurations` offer settings to enable security for data in-transit and data at-rest  
in Amazon Elastic Block Store (Amazon EBS) volumes and EMRFS on Amazon S3.  

Each `security configuration` that you create is stored in Amazon EMR rather than in the cluster configuration,  
so you can easily reuse a configuration to specify data encryption settings whenever you create a cluster  

Data encryption requires keys and certificates.  
A security configuration gives you the flexibility to choose from several options,  
including keys managed by AWS Key Management Service, keys managed by Amazon S3,  
and keys and certificates from custom providers that you supply. 

We need to provide `keys` for encrypting `data at rest` with Amazon EMR, 
We need to provide `certificates` for encrypting `data in transit` with Amazon EMR encryption.  
 
Options for at-rest data encryption include both `Amazon S3 with EMRFS` and `local-disk encryption`.  

# Encryption at rest for EMRFS data in Amazon S3 — 
1. **`Amazon S3 server-side encryption`** —  
When you set up Amazon S3 server-side encryption,  
Amazon S3 encrypts data at the object level as it writes the data to disk and decrypts the data when it is accessed.  
- `SSE-S3` – Amazon S3 manages keys for you
- `SSE-KMS` – You use an AWS KMS customer master key

`SSE with customer-provided keys` (SSE-C) is not available for use with Amazon EMR.  

2. **`Amazon S3 client-side encryption`** —  
With Amazon S3 client-side encryption,  
the Amazon S3 encryption and decryption takes place in the EMRFS client on your cluster.  
Objects are encrypted before being uploaded to Amazon S3 and decrypted after they are downloaded.  

The provider you specify supplies the encryption key that the client uses.  

# Local disk encryption —  

The following mechanisms work together to encrypt local disks  
when you enable local disk encryption using an Amazon EMR security configuration.  

1. **`Open-source HDFS encryption`** —  
HDFS exchanges data between cluster instances during distributed processing.  
It also reads from and writes data to instance store volumes and the EBS volumes attached to instances.  
The following open-source Hadoop encryption options are activated when you enable local disk encryption:  
a.) **Secure Hadoop RPC**  
Setting `hadoop.rpc.protection` to "`privacy`" in the `core-site.xml` activate data encryption.  
b.) **Data encryption on HDFS block data transfer**  
You need to set `dfs.encrypt.data.transfer` to "`true`" in the `hdfs-site.xml`  
in order to activate data encryption for data transfer protocol of DataNode.  
Optionally, you may set `dfs.encrypt.data.transfer.algorithm` to either "3des" or "rc4" to choose the specific encryption algorithm.  



2. **`Instance store encryption`** —  
3. **`EBS volume encryption`** —  

# **`To support encryption with HDFS`** —  
we can use `Transparent encryption in HDFS` on Amazon EMR  
Transparent encryption is implemented through the use of HDFS encryption zones, which are HDFS paths that you define.  
Each encryption zone has its own key,  
which is stored in the key server specified using the hdfs-site configuration classification.  
Amazon EMR uses the Hadoop KMS by default  
HDFS data is encrypted end-to-end (at-rest and in-transit)  
when data is written to an encryption zone because encryption and decryption activities only occur in the client.  
The NameNode and HDFS client interact with the Hadoop KMS


**Reference:**  
1. https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-create-security-configuration.html
2. https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-data-encryption-options.html
3. https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SecureMode.html



