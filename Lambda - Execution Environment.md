# Understanding the Lambda Execution Environment — 

When your functions are invoked,  
the Lambda service runs your code inside an execution environment.  
Lambda scrubs the memory before it is assigned to an execution environment.  
Execution environments are run on hardware virtualized virtual machines (MicroVMs)  
which are dedicated to a single AWS account.  
Execution environments are never shared across functions and MicroVMs are never shared across AWS accounts.  
This is the isolation model for the Lambda service:  

![image](https://user-images.githubusercontent.com/26399543/147839540-ab5dfa8a-9019-46c1-98dd-5d57cf8814e0.png)

A single execution environment may be reused by subsequent function invocations.  
This helps improve performance since it reduces the time taken to prepare an environment.  
Within your code, you can take advantage of this behavior to improve performance further,  
by caching locally within the function or reusing long-lived connections.  
All of these invocations are handled by a single process,  
so any process-wide state (such as static state in Java) is available across all invocations within the same execution environment.  

There is also a local file system available at `/tmp` for all Lambda functions.  
This is local to each function but shared across invocations within the same execution environment.  
If your function must access large libraries or files,  
these can be downloaded here first and then used by all subsequent invocations.  
This mechanism provides a way to amortize the cost and time of downloading this data across multiple invocations.  

While data is never shared across AWS customers,  
it is possible for data from one Lambda function to be shared with another invocation of the same function.  
This may be intended, when used for caching common values or sharing libraries.  
However, if you have information only intended for a single invocation, you should:  
- Ensure that data is only used in a local variable scope.
- Delete any /tmp files before exiting,  
and use UUID naming to prevent different instances from accessing the same temporary files.  
- Ensure that any callbacks are complete before exiting.  

**Reference:**  
1. https://docs.aws.amazon.com/lambda/latest/operatorguide/execution-environment.html

