# To Reduce AWS Costs for `EMR` — 

The cost of our EMR clusters is mainly divided in two with:  
1. `EC2 costs`: the actual machines the cluster runs on,
2. `EMR costs`: the Hadoop service installed on the machines  
including all tools that make managing Hadoop clusters easier.  

![image](https://user-images.githubusercontent.com/26399543/147819902-2771cf7b-9ac2-4f72-95b5-db278bf967e4.png)


Strategy for Cost Reduction -  

1. Use AWS Spot Instances rather than On-Demand Instances whenever possible.  
2. Create a system that shares clusters among several small jobs rather than launching a separate cluster for every job.  
3. Use Amazon EMR tags for cost tracking.  
Using EMR tags lets you track the cost of your cloud usage by project or by department,  
which gives you deeper insight into return on investment and provides transparency for budgeting purposes.  
4. Create a lifecycle management system that allows you to track clusters and eliminate idle clusters.  
5. Use the right instance types for your jobs. For example, **`use c3 instance type for compute-heavy jobs.`**  
This can significantly reduce waste and costs based on the scale of your jobs.  
Below is an algorithm we have found useful  
for selecting the instance type with the best value for compute capacity based on its Spot price:  

  Pseudo Code —  

```python
  maxCpuPerUnitPrice = 0
  optimalInstanceType = null
  for-each instance_type in (Availability Zone, Region) {
    cpuPerUnitPrice = instance.cpuCores/instance.spotPrice
    if (maxCpuPerUnitPrice < cpuPerUnitPrice) {
       optimalInstanceType = instance_type;
    }
  }
```

Good ways to improve the efficiency of your EMR cluster are  
`data partitioning`, `compression`, and `formatting`.  


Some tips/Tricks -  

1. There're 3 aspects like `data-partitioning`, `data-compression` & `data-formatting`  
if we use ORC or Parquet, last 2 of the above aspects get handled,  
- we need to partition the data most commonly by date or any useful keys before storing in S3  
- we should aim to keep partitions larger than 128 MB (the default HDFS block size)  
to avoid the performance hit associated with loading many small files.  
- We can also configure EMR to compress the output of your job, saving bandwidth and storage in both directions.  
- These ORC/Parquet data-formats are optimized for analytics that pre-aggregate metadata about columns.  
2. Use the Right Instance Type -  
The cost of EC2 instances scales with size, so doubling the size of an instance doubles the hourly cost,  
but the cost of managing the EMR overhead for a cluster sometimes remains fixed.  
For many instance families, the hourly EMR fee for an .8xlarge is the same as the hourly EMR fee for a .24xlarge machine.  
This means larger machines running many tasks are more cost efficient,  
they decrease the percentage of your budget spent supporting EMR overhead.  
3. EC2 Pricing -  
on-demand pricing - suitable for master/core node only if we can adopt reserved/saving-plan instances  
reserved instances - results in 30-40 % of cost-reduction but we need to commit the isntances for 1-3 year  
saving plans - are a slightly more flexible version of reserved instances, but we need to commit for 1-3 years
spot-instances - allow clients to purchase unused EC2 capacity at 70-80% cost reduction.  
but it can be claimed at any time with 2 minutes of warning, so, `these aren’t appropriate for most long-running jobs.`  
4. Scaling AWS EMR Clusters - 
EMR utilization also often comes in peaks and valleys of utilization,  
making scaling your cluster a good cost-saving option when handling usage spikes.  
Instance fleets and uniform instance clusters can both use EMR Managed Scaling.  
5. Terminate AWS EMR clusters - 
Terminating clusters after running jobs is great for saving money.  
but auto-terminating clusters also has drawbacks.  
Any data in the HDFS is lost forever upon cluster termination,  
so you will have to write stateless jobs that rely on a metadata store in S3 or Glue.  
Auto-termination is also inefficient when running many small jobs.  
It generally takes less than 15 minutes for a cluster to get provisioned and start processing data,  
but if a job takes 5 minutes to run and you're running 10 in a row, auto-termination quickly takes a toll.  
6. Monitoring AWS EMR Costs - 
 Tag your resources, giving each cluster an owner and business unit to attribute your costs to.  
Tagging is especially important for EMR because it relies on so many other Amazon services.  
It can be really hard to differentiate between the many resources used by the EMR cluster  
when they’re in the same AWS account as your production application.

At high level, 3 general strategy could be like this -  
- Run our workloads on Spot instances.
- Leverage the EMR pricing of bigger EC2 instances.
- Automatically detect idle clusters (and terminate them ASAP).

**Reference:**  
1. https://aws.amazon.com/blogs/big-data/strategies-for-reducing-your-amazon-emr-costs/
2. https://www.cloudforecast.io/blog/aws-emr-cost-optimization-guide/
3. https://www.concurrencylabs.com/blog/guide-to-aws-emr-reserved/
4. https://medium.com/teads-engineering/reducing-aws-emr-data-processing-costs-7c12a8df6f2a

