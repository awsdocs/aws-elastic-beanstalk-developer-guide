# Health Colors and Statuses<a name="health-enhanced-status"></a>

Enhanced health reporting represents instance and overall environment health with four colors, similar to [basic health reporting](using-features.healthstatus.md)\. Enhanced health reporting also provides seven health statuses, single word descriptors that provide a better indication of the state of your environment\.

## Instance Status and Environment Status<a name="health-enhanced-status-type"></a>

Every time Elastic Beanstalk runs a health check on your environment, enhanced health reporting checks the health of each instance in your environment by analyzing all of [the data](health-enhanced.md#health-enhanced-factors) available\. If any of the lower\-level checks fails, Elastic Beanstalk downgrades the health of the instance\.

Elastic Beanstalk displays the health information for the overall environment \(color, status, and cause\) in the [environment management console](environments-console.md)\. This information is also available in the EB CLI\. Health status and cause messages for individual instances are updated every ten seconds and are available from the [EB CLI](eb-cli3.md) when you view health status with [`eb health`](health-enhanced-ebcli.md)\. 

Elastic Beanstalk uses changes in instance health to evaluate environment health, but does not immediately change environment health status\. When an instance fails health checks at least three times in any one\-minute period, Elastic Beanstalk may downgrade the health of the environment\. Depending on the number of instances in the environment and the issue identified, one unhealthy instance may cause Elastic Beanstalk to display an informational message or to change the environment's health status from green \(OK\) to yellow \(Warning\) or red \(Degraded or Severe\)\.

## OK \(Green\)<a name="health-enhanced-status-ok"></a>

An **instance** is passing health checks and the health agent is not reporting any problems\.

Most instances in the **environment** are passing health checks and the health agent is not reporting major issues\.

An **instance** is passing health checks and is completing requests normally\.

Example: Your environment was recently deployed and is taking requests normally\. Five percent of requests are returning 400 series errors\. Deployment completed normally on each **instance**\.

Message \(Instance\): Application deployment completed 23 seconds ago and took 26 seconds\.

## Warning \(Yellow\)<a name="health-enhanced-status-warning"></a>

The health agent is reporting a moderate number of request failures or other issues for an **instance** or **environment**\.

An operation in progress on an **instance** and is taking a very long time\.

Example: One instance in the **environment** has a status of Severe\.

Message \(Environment\): Impaired services on 1 out of 5 instances

## Degraded \(Red\)<a name="health-enhanced-status-degraded"></a>

The health agent is reporting a high number of request failures or other issues for an **instance** or **environment**\.

Example: **Environment** is in the process of scaling up to 5 instances\.

Message \(Environment\): 4 active instances is below Auto Scaling group minimum size 5

## Severe \(Red\)<a name="health-enhanced-status-severe"></a>

The health agent is reporting a very high number of request failures or other issues for an **instance** or **environment**\.

Example: Elastic Beanstalk is unable to contact the load balancer to get instance health\.

Message \(Environment\): *ELB health is failing or not available for all instances\. None of the instances are sending data\. Unable to assume role "arn:aws:iam::123456789012:role/aws\-elasticbeanstalk\-service\-role"\. Verify that the role exists and is configured correctly\.*

Message \(Instances\): Instance ELB health has not been available for 37 minutes\. No data\. Last seen 37 minutes ago\.

## Info \(Green\)<a name="health-enhanced-status-info"></a>

An operation is in progress on an **instance**\.

An operation is in progress on several instances in an **environment**\.

Example: A new application version is being deployed to running instances\.

Message \(Environment\): Command is executing on 3 out of 5 instances

Message \(Instance\): Performing application deployment \(running for 3 seconds\)

## Pending \(Grey\)<a name="health-enhanced-status-pending"></a>

An operation is in progress on an **instance** within the [command timeout](health-enhanced.md#health-enhanced-factors-timeout)\.

Example: You have recently created the environment and **instances** are being bootstrapped\.

Message: Performing initialization \(running for 12 seconds\)

## Unknown \(Grey\)<a name="health-enhanced-status-unknown"></a>

Elastic Beanstalk and the health agent are reporting an insufficient amount of data on an **instance**\.

Example: No data is being received\.