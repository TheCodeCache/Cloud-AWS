# To Reduce AWS Costs - EBS — 

Amazon `Elastic Block Store` (Amazon `EBS`) is the main storage used for EC2 instances.  
It's persistent and hosts both the instance operating system and user data.  

Even if an EBS volume is not attached to any EC2 instance,  
AWS still charges for the amount of data it's persisted into ENS volumes  

Moreover, it doesn't matter how much data is inside the volume.  
For example, if we provision a 500 GB volume, but we only use 10GB,  
then also we will be charged for the whole 500 GBs.  

We can consider below points when it comes to reducing EBS volume costs.  

**`Using the right EBS Type`** —  
There are 4 types of EBS Volumes as follows.

- General Purpose SSD (gp2) Volumes - `this is most commonly used volume types`  
- Provisioned IOPS SSD (io1) Volumes
- Throughput Optimized HDD (st1) Volumes
- Cold HDD (sc1) Volumes  

We should also watch to CloudWatch metrics for the volume to understand,  
whether `IOPS` is getting close to the IOPS limit.  
Note that the minimum size of these volumes is 1 GB and 4 GB respectively.  

`Throughput Optimized HDD` and `Cold HDD` volumes are over `50%` cheaper than the other two.  

They are volumes based on hard disk drives (HDDs).  
They are `good for applications that read or write large volumes of sequential data`,  
like logs of ETL workloads.  
Note that their minimum provisioning size for them is 500 GB.  

`In terms of cost reduction`, the first step is to define the type of volume we need for each instance.  
In case we keep lots of persistent data inside EBS,  
then we should consider migrating it in a Throughput Optimized HDD (st1) or Cold HDD (sc1) Volumes  

**`Migrating to smaller volumes`** —  
AWS considers EBS volumes as `blocks of data`.  
And doesn’t understand the file systems inside them,  
or how data is arranged inside the volume.  
So AWS can increase the volume size, but it can’t reduce the size.  

But, there is a manual process that allows us to migrate a volume to a smaller one.  
The volume has to be mounted in another EC2.  
Then the files have to be copied into a new smaller volume.  
And this new volume replaces will replace the other one.  
We can call this process a volume migration.  

**`Reducing data on volume`** —  
Before migrating to a smaller volume, we need to free some space on it like below.  
- Removing unnecessary files
- Removing unused applications
- Eliminating temporary files or caches  

We could use free tools like `Tree File Size` (for Windows-based OS) or `Disk Usage Analyzer` for Ubuntu,  
or just `ncdu` command in Linux based systems.  

**`Initially deploying small EBS volumes`** —  
This is another strategy to reduce the EBS costs.  
If we need to deploy a new EBS Volume, then try to create a small one.  
And then we can expand the volume size.  
But, let us remember that reducing volume size takes much more work.  

**`Removing unattached volumes`** —  
This is another frequent issue. Clients might have volumes that are unattached.  
They might have terminated their EC2 instance, but forgot to terminate the volume associated.  
These volumes are not used, and still charged daily.  
So consider performing a quick review of all the volumes,  
and defining which are not in use. In order to identify them,  
just check on AWS Console which ones aren't in in-use state  
(for example having 'available' or 'error' states)  

**Using `Delete on Termination`** —  
The `Delete on Termination` is an option we could set when launching the EC2 instance.  
It allows us to automatically remove the EBS volume (attached to an EC2 instance)  
when that instance is terminated. So this avoids keeping an unattached volume.  
We should use it with caution because data on EBS volume will be lost.  


**`Moving data to S3`** —  
Large files in EBS volumes could also be moved to S3.  
For example, Standard S3 rates are 77% below General Purpose SSD (gp2) volume prices per GB.  
Additionally, we pay only for data used, and S3 capacity is doesn’t need to be provisioned.  
And the data is replicated across three AZs at least.

**`Reducing IOPS`** —  
In case we are using Provisioned IOPS SSD (io1) Volumes, we are paying additionally for the IOPS capacity.  
We should check the current IOPS usage using CloudWatch, and compare it with the provisioned amount.  
And this might allow us to reduce the provisioned IOPS rate, and get an extra cost reduction.  

**`Using EC2 Instance Store`** —  
They have a high throughput and very low latency. But these volumes are temporary.  
The data is available only while the EC2 is running.  
When the instance is stopped (or terminated), the data is removed.  
So, these volumes are ideal to keep buffers, caches, and temporary data.  
If we use big amounts of temporary data, we should consider moving them to an instance store volume.  
Keep in mind that only a few EC2 instances types support it.  

**`Avoid Windows-based Operating Systems`** —  
A snapshot is a whole copy of an EBS volume at a certain point in time.  
Snapshots are stored in S3 and replicated across three AZs within the region.  
So, they are highly redundant.  

**`Defining Snapshot Policies`** —  
A snapshot is a whole copy of an EBS volume at a certain point in time.  
Snapshots are stored in S3 and replicated across three AZs within the region. So they are highly redundant.  
Snapshots are also incremental. This means that new backups store only the data that changed since the last snapshot.  
Snapshots price is approximately 50% of the cost EBS storage per GB.  
But these costs could increase fast if we have several snapshots of a volume, or it's data changes fast.  


**Reference:**  
1. https://www.iobasis.com/Strategies-to-reduce-Amazon-EBS-Storage-Costs/

