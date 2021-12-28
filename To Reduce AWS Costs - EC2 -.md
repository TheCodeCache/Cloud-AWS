# Reduce EC2 costs - 

1. Change to “T” family instances:  
It’s used for bursty traffic. For example, when your CPU usage is normally low,  
but with high CPU spikes for short periods of time  
This family of instances is ideal for a website with low traffic.  
these processors can’t have a high CPU usage for long periods.  
2. Use other instance families:  
- `M` families: general purposes applications. Memory to vCPU ratio is `4`  
- `C` families: compute-optimzed, used for applications that need high CPU usage, Memory to vCPU ratio is `2`  
- `R`, `X`, and `Z` families are memory-optimized, Memory-to-vCPU ratio is between 7.6 and 30  
- `P` and `G` families are GPU optimzed,  
- `I`, `D`, and `H` families are storage-optimized.  
3. Change Region  
Some regions are much expensive than others.  
So it's worth thinking about moving your instances to a region with lower prices.  
4. Change the Operating System
When an EC2 instance is running, we are not only paying for the hardware,  
but also for the usage of Operating System.  
hence, choose the OS with less cost  
5. Change to a newer instance
As technology improves, AWS releases new types of instances.  
They have better performance and might have lower costs.  
6. Change instance type based on CPU Usage
Cloudwatch to analyze the CPU usage for each of your instances in the last days.  
If the average CPU usage is below 10%, you should probably consider switching using a smaller instance.  
7. Change instance type based on Memory Usage
- 1st option: Cloudwatch by default doesn’t show the amount of memory used by the instance.  
To find out the memory used, we need log into your instance.  
And check the memory used by the Operating system.  
- 2nd option: check memory usage is to install CloudWatch Agent.  
This agent collects some metrics of your instance and sends them to CloudWatch.  
Then we can see all the metrics inside the AWS CloudWatch console without logging into the instance.  
And they are saved for some time.  
8. Use instances Auto-Scaling
We might need to check on aggressive scaling, etc.  
9. Use an AWS managed service
Some AWS service might perform the same type of service that we are executing on our own on EC2.  
In such cases, let us switch to AWS services, like RDS, SFTP, ElastiCache etc.   
10. Use Spot Instances  
EC2 instances can be purchased in 5 ways -   
- OnDemand Instances
- Spot instances
- Saving Plans
- Reserve Instances
- Dedicated Hosts
Spot instances are the most economical of all. We can easily get a 70% discount over OnDemand prices  
11. Use Savings Plans & Reserved Instances  
this allows for saving money by committing to use EC2s for 1 or 3 years.  

**Reference:**  
1. https://www.iobasis.com/Strategies-to-reduce-Amazon-EC2-Instances-Costs/

