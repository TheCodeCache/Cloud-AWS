# Lambda Scaling — 

The first time you invoke your function,  
AWS Lambda creates an instance of the function and runs its handler method to process the event.  
When the function returns a response, it stays active and waits to process additional events.  
If you invoke the function again while the first event is being processed,  
Lambda initializes another instance, and the function processes the two events concurrently.  
As more events come in, Lambda routes them to available instances and creates new instances as needed.  
When the number of requests decreases,  
Lambda stops unused instances to free up scaling capacity for other functions.  

# Lambda quotas —  

Compute and Storage:  

|Resource|Default Quota|
|---|---|
|Concurrent executions|1000|
|Size of all functions| 75 GB|

![image](https://user-images.githubusercontent.com/26399543/147839357-134da9f4-417d-4be4-ae3b-b0908db53ef4.png)

![image](https://user-images.githubusercontent.com/26399543/147839363-8b32a662-428e-4490-b52c-820f4b9714ee.png)

![image](https://user-images.githubusercontent.com/26399543/147839370-65b926bd-ca6d-45bc-8d8d-c0552f5db307.png)

**Reference:**  
1. https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html

