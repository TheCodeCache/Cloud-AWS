# Should we put Lambda inside VPC ? — 

[note](https://docs.aws.amazon.com/lambda/latest/operatorguide/networking-vpc.html): `Lambda functions always run inside VPCs owned by the Lambda service`  
These VPCs are not visible to customers, the configurations are maintained automatically,  
and monitoring is managed by the service.  

[note](https://docs.aws.amazon.com/lambda/latest/operatorguide/networking-vpc.html):  
When we create a Lambda function without a VPC configuration,  
it’s automatically available in all Availability Zones within the Region.  
When we set up VPC access, we choose which Availability Zones the Lambda function can use.  
As a result, to provide continued high availability,  
ensure that the function has access to at least two Availability Zones.  

**Summary:** `Don't put the Lambda function in a VPC unless we have to`  

**Explanation:**  

When we create a Lambda function, we can give the function access to one of the VPCs  
Additionally, we need to specify what security groups and subnets to use for the VPC connection  

However, these choices can have a big impact on performance and scalability.  

For a function to access resources inside the designated VPC, it needs an Elastic Network Interface (ENI).  
ENIs are expensive and time-consuming to create, so AWS has implemented a number of optimizations.  

First of all, ENIs are shared across containers.  
If a function is allocated with 1GB of memory,  
then up to three (3) containers running this function can share the same ENI  

![image](https://user-images.githubusercontent.com/26399543/147832810-c6ddb51c-8a61-451a-a183-2c30cd2087d1.png)

Idle containers are usually garbage collected after a few minutes.  
VPC-enabled functions are allowed to stay idle for longer to increase the likelihood that existing ENis are reused.  

However, even with these optimizations,  
VPCs still impose serious scalability and performance limitations on Lambda functions.  

**Touble in VPC landscape** —   

**ENI Limits:**  
By default, there is a soft limit of 350 ENIs per region.  
If this limit is reached then invocations of VPC-enabled functions will be throttled  

that's why we should always ask for a limit raise for ENIs whenever we need to ask for a concurrency raise for Lambda.  

![image](https://user-images.githubusercontent.com/26399543/147832871-770fe43c-8fc3-4a7b-895f-9bcdd46d2b82.png)

**IP Limits:**  
Each ENI consumes a private IP address from the associated subnet,  
and each subnet has a limited number of available IP addresses based on its CIDR block.  
It's therefore possible to exhaust the available IP addresses in a subnet when Lambda functions scale up quickly.  
When this happens, Lambda would not be able to scale up the number of concurrent executions.  
This would impact other services such as EC2 or RDs,  
which also requires available IP addresses for ENIs.  
Therefore, it's recommended that we create dedicated subnets with large IP ranges for the Lambda functions.  

**Loss of internet access:**  
A VPC-enabled function would lose internet access because its ENI is only associated with a private IP address from the subnet.  
If the lambda function needs to talk to other AWS services that are outside the VPC,  
such as DynamoDB or SNS, then it needs to have internet access.  
In this case, we need a `NAT instance` or an `Amazon NAT Gateway`, which adds additional cost and complexity.  

However, as an alternative, we can use `VPC endpoints` to communicate with other AWS services instead.  
However, not every AWS service is supported, and VPC endpoints are also more complicated to set up and use.  

**Slow cold starts:**  
The **`biggest drawback`** of VPC-enabled functions is that they suffer significantly longer cold starts.  
Because the process of ENIs is time-consuming and can often take up to 10s or more.  

Below is a comparison of the average cold start time for function inside and outside VPC.  

![image](https://user-images.githubusercontent.com/26399543/147833041-ac64dd93-b023-40ac-9501-0f64ccd2ebf3.png)

**Lambda Security Aspect:**  

it does not require a VPC to secure Lambda,  
`Lambda functions are protected by AWS Identity and Access Management (IAM) service`,  
which provides both authentication and authorization.  
All incoming requests have to be signed with a valid AWS credential,  
and the caller must have the correct IAM permission.  

This is the same mechanism that protects most other AWS services

The EC2 instances that host our Lambda functions are not publicly accessible.  
It is therefore also not possible for attackers to compromise our functions by compromising the host  

**Protecting outbound traffic:**  
VPCs allow us to control outbound traffic using security groups and outbound rules.  
It is, therefore, possible to stop attackers from leaking sensitive data to the internet.  

However, we rarely see egress filtering actually enforced in the real world.  
Therein lies a common fallacy with VPCs.  

# Very Important From Security Perspective 

**`They give us a false sense of confidence that attackers cannot get inside the VPC  
and therefore everything inside the VPC is given full trust.  
This is of course not the case. Even though malicious ingress traffic can be stopped by IAM and VPC,  
it’s still possible to compromise Lambda functions via other means.  
For example, it’s possible to compromise a function  
through its dependencies and steal sensitive data such as AWS credentials.`**  

# When should we use VPC? 

if the lambda function needs to access a resource that runs in the VPC,  
for example:  
- RDS database
- Elasticache cluster
- Internal APIs (running on containers or EC2)

![image](https://user-images.githubusercontent.com/26399543/147837805-37a25cf1-f577-458b-bb5e-ab60529030ba.png)

**Summary:**  
VPCs don't provide the same level of security to Lambda as they do for EC2.  
Because, Lambda functions are already secured by IAM, which provides both authentication and authorization.  
IAM stops malicious inbound traffic, but we still need to protect against malicious outbound traffic  
We can use tools such as `FunctionShield` to block connectivity to public internet without resorting to VPCs,  
which helps us stop attackers from stealing sensitive data from compromised functions.  

On the flip side, VPCs introduce a host of challenges to Lambda:  
- VPC-enabled functions experience drastically longer cold start duration.
- We can run into the regional ENI limits.
- We can exhaust available IP.

Hence, in general, it's better to stay away from VPCs as much as possible!  

**Reference:**  
1. https://medium.com/lumigo/to-vpc-or-not-to-vpc-pros-and-cons-in-aws-lambda-aeaad000432

