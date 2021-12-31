# Best Practices to work with AWS Lambda — 

The following are recommended best practices for using AWS Lambda:  

1. **Function code** —  
- Separate the Lambda handler from your core logic,  
for ex:  
```javascript
exports.myHandler = function(event, context, callback) {
	var foo = event.foo;
	var bar = event.bar;
	var result = MyLambdaFunction (foo, bar);

	callback(null, result);
}

function MyLambdaFunction (foo, bar) {
	// MyLambdaFunction logic here
}
```
- Take advantage of execution environment reuse to improve the performance of your function,  
Initialize SDK clients and database connections outside of the function handler,  
and cache static assets locally in the /tmp directory.  
Subsequent invocations processed by the same instance of your function can reuse these resources.  
This saves cost by reducing function run time.  
- Use a keep-alive directive to maintain persistent connections,  
Lambda purges idle connections over time. Attempting to reuse an idle connection  
when invoking a function will result in a connection error.  
To maintain your persistent connection, use the keep-alive directive associated with your runtime  
```javascript
const AWS = require('aws-sdk');
// http or https
const http = require('http');
const agent = new http.Agent({
  keepAlive: true, 
// Infinity is read as 50 sockets
  maxSockets: Infinity
});

AWS.config.update({
  httpOptions: {
    agent
  }
});
```
- **Use environment variables to pass operational parameters to your function**,  
For example, if we are writing to an Amazon S3 bucket,  
instead of hard-coding the bucket name we are writing to, configure the bucket name as an environment variable.  
- **Control the dependencies in your function's deployment package**,  
Instead of relying on AWS Lambda's supported library,  
as the version changes might have unexpected bahvior to our lambda function   
hence, let us zip all the dependency required for our lambda code and deploy it along with lambda code,  
- **Minimize your deployment package size to its runtime necessities**.  
- Reduce the time it takes Lambda to unpack deployment packages authored in Java by putting your dependency  
.jar files in a separate /lib directory.  
This is faster than putting all this function’s code in a single jar with a large number of .class files  
- **Minimize the complexity of your dependencies**.  
Prefer simpler frameworks that load quickly on execution environment startup.  
For example, prefer simpler Java dependency injection (IoC) frameworks like Dagger or Guice,  
over more complex ones like Spring Framework
- **Avoid using recursive code:**  
This could lead to unintended volume of function invocations and escalated costs.  
If you do accidentally do so, set the function reserved concurrency to 0 immediately  
to throttle all invocations to the function, while you update the code.  

2. **Function configuration**  —  
- Performance testing your Lambda function,  
This is a crucial part in ensuring we pick the optimum memory size configuration.  
Any increase in memory size triggers an equivalent increase in CPU available to your function  
The memory usage for your function is determined per-invoke and can be viewed in Amazon CloudWatch  
```log
REPORT RequestId: 3604209a-e9a3-11e6-939a-754dd98c7be3	Duration: 12.34 ms	Billed Duration: 100 ms Memory Size: 128 MB	Max Memory Used: 18 MB
```
To find the right memory configuration for your functions,  
AWS recommends using the open source AWS Lambda `Power Tuning` project.  
- Load test your Lambda function  
this is to determine an optimum timeout value.  
This is especially important when your Lambda function makes network calls to resources that may not handle Lambda's scaling.
- Use most-restrictive permissions when setting IAM policies  
Understand the resources and operations your Lambda function needs, and limit the execution role to these permissions  
- Be familiar with Lambda quotas.  
Payload size, file descriptors and /tmp space are often overlooked when determining runtime resource limits.  
- Delete Lambda functions that you are no longer using  
- Using with Amazon Simple Queue Service as an event source,  
make sure the value of the function's expected invocation time does not exceed the Visibility Timeout value on the queue.  
This applies both to CreateFunction and UpdateFunctionConfiguration.  
  - In the case of CreateFunction, AWS Lambda will fail the function creation process.  
  - In the case of UpdateFunctionConfiguration, it could result in duplicate invocations of the function.  

3. **Metrics and Alarms* —  
- Use Working with Lambda function metrics and CloudWatch Alarms  
instead of creating or updating a metric from within your Lambda function code  
- Test with different batch and record sizes  
so that the polling frequency of each event source is tuned to how quickly your function is able to complete its task  
- Increase Kinesis stream processing throughput by adding shards  
A Kinesis stream is composed of one or more shards.  
Lambda will poll each shard with at most one concurrent invocation.  
For example, if your stream has 100 active shards, there will be at most 100 Lambda function invocations running concurrently.  
- Use Amazon CloudWatch on IteratorAge to determine if your Kinesis stream is being processed.  


**Reference:**  
1. https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html

