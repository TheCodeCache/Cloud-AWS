# AWS `Blue/Green` Deployment: 

While performing application version update, there are different technics available. Blue/Green Deployment is one of them.  
The other one is In-Place Deployment with which the application may become unavailable while deployment.  
To avoid that downtime, Blue/Green Deployment is one of the best options.  

In Blue/Green Methodology,  
there are 2 environments which are independent of each other. The environments may be identical or may be incompatible platform versions.  
This naming convention Blue/Green is to differentiate between the two environments.  
Blue is the current environment and Green is the one with updated application version.  

![image](https://user-images.githubusercontent.com/26399543/147497539-93ee46db-336b-45bd-a737-42b7c2c5e43f.png)

`Blue/Green deployment can be achieved either by modifying DNS or by using Load Balancer`  

Canary Testing can also be implemented in Blue/Green deployment where a small percentage of traffic is shifted to Green environment.  
Testing is done with new code and once signed off, the whole traffic is shifted to Green environment.  
Elastic Load Balancer plays a major role in such cases by automatically scaling the request handling capacity to manage the traffic.  

**How can it be achieved ?**  
1. `Using Elastic Load Balancer`  
  Elastic Load Balancing is a mechanism with which traffic is distributed across multiple targets.  
  This is one of best techniques to implement Blue/Green Deployment.  
  It helps in distributing the traffic across Blue (current environment) and Green (new environment) as required  
2. `Using AWS Elastic Beanstalk`  
  AWS Elastic Beanstalk is AWS service for quickly deploying and managing web applications and services developed in Java, .NET, Python, Node.js, PHP, Ruby, Go and Docker.  
  Elastic Beanstalk provides URL of the running application.  
  In case of Blue/Green deployment, the current application URL works as Blue environment and the new updated Application URL works as Green environment.  
  To deploy Green environment, Elastic Beanstalk provides an option of Swap Environment URL.  
  The same option can be taken again to roll back the changes to old application version.  
3. `Using Auto Scaling`  
  Auto Scaling facilitates to increase or decrease the number of instances to the application as required.  
  To enable Blue/Green Deployment, auto scaling group can be attached to different Launch Configurations  
4. `Using Amazon Route 53`  
  Amazon Route 53 is highly available and scalable Domain Name System (DNS) web service.  
  It facilitates to buy domain, route traffic by configuring the records for different services  
  It also has feature to allow certain percentage of traffic to be given to each server.  
  This feature is helpful in implementing Blue/Green Deployment.  
  Gradually whole traffic is shifted from Blue to Green environment.  
  This kind of traffic management is known as Canary deployment.  
  This is one of the effective features which can help in easy rollback too.  

**Advantages:**  
- Blue/Green deployment methodology is one of the new techniques introduced.  
  In this technique it requires nearly zero downtime for switching the environments.  
  This is mainly useful for highly critical applications which cannot accept downtime.  
- It helps in running old version available in Blue environment and at the same time validating the new version on Green environment.  
  If any discrepancy is found in Green environment, it can be switched back to Blue environment that was untouched.  
- In case rollback is required, it can be done hassle-free to old version with nearly zero downtime.  
  This will have least chances of introducing errors too.
- It allows faster disaster recovery and reduces the risks which are there with updates.

**When not to use Blue/Green Deployment:**  
- As Blue/Green Deployment is expensive so when cost is also a major factor, it should not be used.  
- When there are many external integrations, it might be difficult to map/integrate all the services again.  
- When there is complex application it might be difficult to handle migration of running instances/schema.  
- In case of transaction critical application, Blue-Green switching must be handled carefully else the transaction detail might get lost.  
- Many software products come with their own update and upgrade process so in such cases Blue/Green Deployment is not suitable.

# Best Practices - 

To manage data across 2 different environments (Blue and Green) takes lots of effort in maintaining data consistency.  
To handle that, there are few best practices which minimizes the risk:  
- Schema changes should be out of deployment box.
- All required connections of Blue environment to be closed once that is switched to Green environment.

Application deployment involves risks, but deployment technics like Blue/Green helps reduce human errors, downtime and mitigate risks.  

**Reference:**  
1. https://impetusinfotech.sharepoint.com/sites/BloggingPlatform/SitePages/AWS-Blue-Green-Deployment.aspx

