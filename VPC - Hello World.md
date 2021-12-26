# Build a basic VPC (Virtual Private Cloud) in AWS 

![image](https://user-images.githubusercontent.com/26399543/147407357-d423c0ab-2adb-4843-ab80-8c5ba46e8fc3.png)

Basically, There're 6 components in general as follows:  

1. VPC
2. Subnets
3. Security Group (SG)
4. NACL (N/W Access Control List)
5. Routing Tables
6. Internet Gateway (IG)

`Security Group` and `NACL` both are security layers within VPC.  
`SG` acts as a firewall at individual EC2 instance level,  
`NACL` acts as a firewall at subnet level  

And all of the above can be accomplished via AWS VPC console.  

**Reference:**  
1. https://www.whizlabs.com/blog/create-virtual-private-cloud-in-aws/

