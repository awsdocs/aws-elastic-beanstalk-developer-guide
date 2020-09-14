# Basic health reporting<a name="using-features.healthstatus"></a>

AWS Elastic Beanstalk uses information from multiple sources to determine if your environment is available and processing requests from the Internet\. An environment's health is represented by one of four colors, and is displayed on the [environment overview](environments-console.md) page of the Elastic Beanstalk console\. It's also available from the [DescribeEnvironments](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironments.html) API and by calling eb status with the [EB CLI](eb-cli3.md)\.

Prior to version 2 Linux platform versions, the only health reporting system was basic health\. The basic health reporting system provides information about the health of instances in an Elastic Beanstalk environment based on health checks performed by Elastic Load Balancing for load\-balanced environments, or Amazon Elastic Compute Cloud for single\-instance environments\.

In addition to checking the health of your EC2 instances, Elastic Beanstalk also monitors the other resources in your environment and reports missing or incorrectly configured resources that can cause your environment to become unavailable to users\.

Metrics gathered by the resources in your environment is published to Amazon CloudWatch in five minute intervals\. This includes operating system metrics from EC2, request metrics from Elastic Load Balancing\. You can view graphs based on these CloudWatch metrics on the [Monitoring page](environment-health-console.md) of the environment console\. For basic health, these metrics are not used to determine an environment's health\.

**Topics**
+ [Health colors](#using-features.healthstatus.colors)
+ [Elastic Load Balancing health checks](#using-features.healthstatus.understanding)
+ [Single instance and worker tier environment health checks](#monitoring-basic-healthcheck-singleinstance)
+ [Additional checks](#monitoring-basic-additionalchecks)
+ [Amazon CloudWatch metrics](#monitoring-basic-cloudwatch)

## Health colors<a name="using-features.healthstatus.colors"></a>

Elastic Beanstalk reports the health of a web server environment depending on how the application running in it responds to the health check\. Elastic Beanstalk uses one of four colors to describe status, as shown in the following table:


****  

| Color | Description | 
| --- | --- | 
|  Grey  | Your environment is being updated\. | 
|  Green  |  Your environment has passed the most recent health check\. At least one instance in your environment is available and taking requests\.  | 
|  Yellow  |  Your environment has failed one or more health checks\. Some requests to your environment are failing\.  | 
|  Red  |  Your environment has failed three or more health checks, or an environment resource has become unavailable\. Requests are consistently failing\.  | 

These descriptions only apply to environments using basic health reporting\. See [Health colors and statuses](health-enhanced-status.md) for details related to enhanced health\.

## Elastic Load Balancing health checks<a name="using-features.healthstatus.understanding"></a>

In a load\-balanced environment, Elastic Load Balancing sends a request to each instance in an environment every 10 seconds to confirm that instances are healthy\. By default, the load balancer is configured to open a TCP connection on port 80\. If the instance acknowledges the connection, it is considered healthy\.

You can choose to override this setting by specifying an existing resource in your application\. If you specify a path, such as `/health`, the health check URL is set to `HTTP:80/health`\. The health check URL should be set to a path that is always served by your application\. If it is set to a static page that is served or cached by the web server in front of your application, health checks will not reveal issues with the application server or web container\. For instructions on modifying your health check URL, see [Health check](environments-cfg-clb.md#using-features.managing.elb.healthchecks)\.

If a health check URL is configured, Elastic Load Balancing expects a GET request that it sends to return a response of `200 OK`\. The application fails the health check if it fails to respond within 5 seconds or if it responds with any other HTTP status code\. After 5 consecutive health check failures, Elastic Load Balancing takes the instance out of service\. 

For more information about Elastic Load Balancing health checks, see [Health Check](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/TerminologyandKeyConcepts.html#healthcheck) in the *Elastic Load Balancing User Guide*\.

**Note**  
Configuring a health check URL does not change the health check behavior of an environment's Auto Scaling group\. An unhealthy instance is removed from the load balancer, but is not automatically replaced by Amazon EC2 Auto Scaling unless you configure Amazon EC2 Auto Scaling to use the Elastic Load Balancing health check as a basis for replacing instances\. To configure Amazon EC2 Auto Scaling to replace instances that fail an Elastic Load Balancing health check, see [Auto Scaling health check setting](environmentconfig-autoscaling-healthchecktype.md)\.

## Single instance and worker tier environment health checks<a name="monitoring-basic-healthcheck-singleinstance"></a>

In a single instance or worker tier environment, Elastic Beanstalk determines the instance's health by monitoring its Amazon EC2 instance status\. Elastic Load Balancing health settings, including HTTP health check URLs, cannot be used in these environment types\.

For more information on Amazon EC2 instance status checks, see [Monitoring Instances with Status Checks](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

## Additional checks<a name="monitoring-basic-additionalchecks"></a>

In addition to Elastic Load Balancing health checks, Elastic Beanstalk monitors resources in your environment and changes health status to red if they fail to deploy, are not configured correctly, or become unavailable\. These checks confirm that:
+ The environment's Auto Scaling group is available and has a minimum of at least one instance\.
+ The environment's security group is available and is configured to allow incoming traffic on port 80\.
+ The environment CNAME exists and is pointing to the right load balancer\.
+ In a worker environment, the Amazon Simple Queue Service \(Amazon SQS\) queue is being polled at least once every three minutes\.

## Amazon CloudWatch metrics<a name="monitoring-basic-cloudwatch"></a>

With basic health reporting, the Elastic Beanstalk service does not publish any metrics to Amazon CloudWatch\. The CloudWatch metrics used to produce graphs on the [Monitoring page](environment-health-console.md) of the environment console are published by the resources in your environment\.

For example, EC2 publishes the following metrics for the instances in your environment's Auto Scaling group:

`CPUUtilization`  
Percentage of compute units currently in use\.

`DiskReadBytes``DiskReadOps``DiskWriteBytes``DiskWriteOps`  
Number of bytes read and written, and number of read and write operations\.

`NetworkIn``NetworkOut`  
Number of bytes sent and received\.

Elastic Load Balancing publishes the following metrics for your environment's load balancer:

`BackendConnectionErrors`  
Number of connection failures between the load balancer and environment instances\.

`HTTPCode_Backend_2XX``HTTPCode_Backend_4XX`  
Number of successful \(2XX\) and client error \(4XX\) response codes generated by instances in your environment\.

`Latency`  
Number of seconds between when the load balancer relays a request to an instance and when the response is received\.

`RequestCount`  
Number of completed requests\.

These lists are not comprehensive\. For a full list of metrics that can be reported for these resources, see the following topics in the Amazon CloudWatch Developer Guide:


**Metrics**  

| Namespace | Topic | 
| --- | --- | 
| AWS::ElasticLoadBalancing::LoadBalancer | [Elastic Load Balancing Metrics and Resources](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/elb-metricscollected.html) | 
| AWS::AutoScaling::AutoScalingGroup | [Amazon Elastic Compute Cloud Metrics and Resources](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/ec2-metricscollected.html) | 
| AWS::SQS::Queue | [Amazon SQS Metrics and Resources](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/sqs-metricscollected.html) | 
| AWS::RDS::DBInstance | [Amazon RDS Dimensions and Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/rds-metricscollected.html) | 

### Worker environment health metric<a name="w9aac25b9c21c18"></a>

For worker environments only, the SQS daemon publishes a custom metric for environment health to CloudWatch, where a value of 1 is Green\. You can review the CloudWatch health metric data in your account using the `ElasticBeanstalk/SQSD` namespace\. The metric dimension is `EnvironmentName`, and the metric name is `Health`\. All instances publish their metrics to the same namespace\.

To enable the daemon to publish metrics, the environment's instance profile must have permission to call `cloudwatch:PutMetricData`\. This permission is included in the default instance profile\. For more information, see [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)\. 