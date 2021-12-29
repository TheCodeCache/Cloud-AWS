# Cloud Migration - 

>AWS Migration is the process of moving data, applications, or other business components  
from an organization's `on-premises` infrastructure to the `cloud`,  
or moving them from one cloud service to another.  

>In order to understand the basics of migrating to cloud, we need to understand the application in depth.  

>There're different strategies, which companies take to identify which application needs to be retired  
>and which application needs to go through a migration phase, there're different migration phases 


Cloud migration has always been an interesting, challenging and cautious process.  
There are varieties of applications in the world of technology. There are some applications -  

1. which are not designed for cloud and works best on-premises.  
2. with few minor changes can be easily lifted and shifted on cloud.  
3. that can’t be easily lifted and shifted to the cloud.  
New infrastructure can adversely affect performance of such application.  

the challenge is to migrate on-premise workloads to cloud without any customer impact or downtime,  
and there're certain factors to consider for cloud migration.  

some key pillars of cloud migration processess are -  

![image](https://user-images.githubusercontent.com/26399543/147709208-26c860cc-3b0c-4c04-83eb-b0016bd9da13.png)

**`Vision`**  
Typically every workloads should be highly available, scalable, secured, managed and enterprise grade,  
Try to comprehend the vision of customer in terms of the above factors as well as  
its long term goals like the solution should sustain for the next 5 years of business growth etc.  

We could apply appropriate cloud migration strategies -  
(like - 5R’s rehost, refactor, revise, rebuild and replace)  as applicable during migration process  

**`Prerequisites & Readiness`**  
Before starting cloud migration,  
first we need to understand and asses our application footprints and business critical information.  
Using below pointers, we can bring velocity, reliability & visibility  
in cloud migration process as well as can also avoid issues related to  
SLAs violation, security breach, non-functional issues related to performance, scalability and cost etc.  

- Team Mentoring - Coaching of team on cloud technologies.
- Security and Compliance - Validate any security risk of moving client's application data on cloud.  
Some enterprises protect and secure their data assets by not allowing data to go outside of their country or region boundaries.  
- Application Assessment  
  - Map applications with third party dependencies including databases, data transfer rate and size among various application.  
  - Build, release and deployment topology of applications shall be clearly defined.
  - Enough coverage of regression suite for validating functional behavior.
  - Sanity suite for validating successful deployment.  
- Return on Investment -  
Calculate the application hardware, network, licensing costs including operational and maintenance efforts.  
- Operational & Capacity Assessment -  
  - Alerting and monitoring process of all applications. Monitor Services statistics & utilization.  
  - Continuous integration and automated deployment.
  - Application runbook that contains detailed documentation to keep the application operational.
  - Asses mission critical metrices of the business SLAs like response time, application/data availability & scalability, acceptable application downtime etc.  
  - Pick the proper size of resources with auto scale up/down as per service usage
  - Plan for future growth
  - Asses application usage metrices like numbers of concurrent users of the application, geographic location of users, frequently and widely user applications.  

**`Hosting Strategy & Service Provider Analysis`**  
It is always better to determine the hosting strategy as cloud agnostic for avoiding vendor lock-in,  
We can opt Public, Private, Hybrid or Multi cloud as the hosting strategy as per application needs.  

Picking a right service provider is the key aspect during cloud migration.  
There are many cloud providers in the market (AWS, Google, Azure, Oracle).  
Evaluate cloud service provider on parameters  
like cost, SLAs, scalability, availability, geographic coverage, security compliance,  
help & support, operational maintenance, application migration tools etc.  

For our client migration, we opted hybrid cloud hosting strategy. We hosted services on our own private cloud,  
where we had requirement of low latency, custom route announcements and legal compliance.  
Remaining services were hosted on public/private cloud on AWS.  

**`Application & Data Migration Strategy`**  
As per application design and architecture, it is important to choose appropriate migration strategy.  
As per application needs, we can decide to choose Rehost (Lift & Shift) or Refactor or Revise or Rebuild or Replace as cloud migration strategy.  

Start with first migration of applications which are having least dependencies or are less complex.  
Business critical applications with higher dependencies are always recommended to migrate at the later phases.  

- Initially we started with Lift & Shift of services along with database on cloud.  
This opens doors for us to utilize cloud services more effectively.  
- Refactor some of the services to eliminate single point of failure and make them stateless with loose coupling between services.  
- At later phases, start rebuilding and replacing existing services with cloud-native services  
to gain more benefits around capacity, availability, faster development, low operational cost etc.  

Below are few considerations, that needs to be taken while building migration strategy –  

- Asses application migration plan in staggered fashion to avoid larger impact on business.
- Customer which are having very low traffic or application usage can be migrate first on cloud  
whereas customers who uses applications heavily can be considered for later migration phases.  
- Draft rollback strategy in case of any disaster or catastrophic situation.
- Evaluate and identify migration window which has very little impact to business.
- Plan customer notifications in advance to proactively inform to customers around impacted service disruption if any.
- Setup internal and external monitoring of the applications health check and resources.
- Setup auto scaling up/down of the resources as per their usage to leverage resources effectively.
- Setup policies for rotating applications, database passwords, access keys every few weeks or months.
- Have additional checks before exposing anything to internet (do not expose any sensitive information on 0.0.0.0 to the world).
- Have redundancy of the services and setup data backup policies of business-critical information.
- Use private IPs for any communication within cloud to avoid traffic on internet which can incur extra cost and increase network latency.
- Setup auto recovery policies of critical infrastructure on cloud for business continuity in case of any hardware failure.

**`Data Migration strategy`**  
During application migration, location of the database plays a significant impact on performance of application.  
If the database is still on-premise, moving the application on cloud can have adverse effect on performance.  
We can set up a new database on cloud so that applications on cloud can connect directly to cloud database.  
`Replication` can be setup between on-premise database and cloud database,  
so that database is synchronized and up-to date.  
Once we move applications to cloud, we can decommission on-premise database.  

With this data migration plan,  
we can be able to keep data transfer cost to very minimum with no impact on application performance.  
Adding more storage space and vertical scaling of database becomes very easy after migration to cloud.  

**`Migration Validation`**  
This is one of key factor in entire cloud migration journey.  
Success of application cloud migration is heavily relying on cloud migration validation phase.  
We have created wide variety of metrices for tracking all non-functional business parameters.  
We have used single click deployment model along with blue/green deployment technique  
to eliminate downtime due to application deployment.  
Validate application migration on parameters  
like functional parity, performance, SLAs, alerting & monitoring, security & compliance etc.  

# `Conclusion`
Cloud migration is often considered to be very difficult and challenging process.  
However, it becomes relatively easy & smooth if executed with proper assessment & planning.  

**Reference:**  
1. https://impetusinfotech.sharepoint.com/sites/BloggingPlatform/SitePages/Key-Pillars-of-Cloud-Migration.aspx

