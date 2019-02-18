# Enhanced Health Reporting and Monitoring<a name="health-enhanced"></a>

Enhanced health reporting is a feature that you can enable on your environment to allow AWS Elastic Beanstalk to gather additional information about resources in your environment\. Elastic Beanstalk analyzes the information gathered to provide a better picture of overall environment health and aid in the identification of issues that can cause your application to become unavailable\.

In addition to changes in how health color works, enhanced health adds a *status* descriptor that provides an indicator of the severity of issues observed when an environment is yellow or red\. When more information is available about the current status, you can choose the **Causes** button to view detailed health information on the [health page](health-enhanced-console.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-dashboard-cause.png)

To provide detailed health information about the EC2 instances running in your environment, Elastic Beanstalk includes a [health agent](#health-enhanced-agent) in the Amazon Machine Image \(AMI\) for each platform version that supports enhanced health\. The health agent monitors web server logs and system metrics and relays them to the Elastic Beanstalk service\. Elastic Beanstalk analyzes these metrics along with data from Elastic Load Balancing and Amazon EC2 Auto Scaling to provide an overall picture of an environment's health\.

In addition to collecting and presenting information about your environment's resources, Elastic Beanstalk monitors the resources in your environment for several error conditions and provides notifications to help you avoid failures and resolve configuration issues\. [Factors that influence your environment's health](#health-enhanced-factors) include the results of each request served by your application, metrics from your instances' operating system, and the status of the most recent deployment\.

You can view health status in real time by using the [environment dashboard](health-enhanced-console.md) in the AWS Management Console or the [eb health](health-enhanced-ebcli.md) command in the [Elastic Beanstalk command line interface](eb-cli3.md) \(EB CLI\)\. To record and track environment and instance health over time, you can configure your environment to publish the information gathered by Elastic Beanstalk for enhanced health reporting to Amazon CloudWatch as custom metrics\. CloudWatch [charges](https://aws.amazon.com/cloudwatch/pricing/) for custom metrics apply to all metrics other than `EnvironmentHealth`, which is free of charge\.

Enhanced health reporting requires a version 2 or newer [ platform version](concepts.platforms.md) and is supported by all platforms except Windows Server with IIS\. In order to monitor resources and publish metrics, your environment must have both an [instance profile and service](#health-enhanced) role\. The Multicontainer Docker platform doesn't include a web server by default but can be used with enhanced health reporting if you configure your web server to [provide logs in the proper format](health-enhanced-serverlogs.md)\.

The first time you create an environment with a version 2 platform version in the AWS Management Console, Elastic Beanstalk prompts you to create the required roles and enables enhanced health reporting by default\. Continue reading for details on how enhanced health reporting works, or go to [Enabling AWS Elastic Beanstalk Enhanced Health Reporting](health-enhanced-enable.md) to get started using it right away\.

**Topics**
+ [The Elastic Beanstalk Health Agent](#health-enhanced-agent)
+ [Factors in Determining Instance and Environment Health](#health-enhanced-factors)
+ [Health Check Rule Customization](#health-enhanced.rules)
+ [Enhanced Health Roles](#health-enhanced-roles)
+ [Enhanced Health Events](#health-enhanced-events)
+ [Enhanced Health Reporting Behavior During Updates, Deployments, and Scaling](#health-enhanced-effects)
+ [Enabling AWS Elastic Beanstalk Enhanced Health Reporting](health-enhanced-enable.md)
+ [Enhanced Health Monitoring with the Environment Management Console](health-enhanced-console.md)
+ [Health Colors and Statuses](health-enhanced-status.md)
+ [Instance Metrics](health-enhanced-metrics.md)
+ [Configuring Enhanced Health Rules for an Environment](health-enhanced-rules.md)
+ [Publishing Amazon CloudWatch Custom Metrics for an Environment](health-enhanced-cloudwatch.md)
+ [Using Enhanced Health Reporting with the AWS Elastic Beanstalk API](health-enhanced-api.md)
+ [Enhanced Health Log Format](health-enhanced-serverlogs.md)
+ [Notifications and Troubleshooting](environments-health-enhanced-notifications.md)

## The Elastic Beanstalk Health Agent<a name="health-enhanced-agent"></a>

The Elastic Beanstalk health agent is a daemon process that runs on each EC2 instance in your environment, monitoring operating system and application\-level health metrics and reporting issues to Elastic Beanstalk\. The health agent is included in all Linux platform versions starting with version 2\.0 of each platform\.

The health agent reports similar metrics to those [published to CloudWatch](using-features.healthstatus.md#monitoring-basic-cloudwatch) by Amazon EC2 Auto Scaling and Elastic Load Balancing as part of [basic health reporting](using-features.healthstatus.md), including CPU load, HTTP codes, and latency\. The health agent, however, reports directly to Elastic Beanstalk, with greater granularity and frequency than basic health reporting\.

For basic health, these metrics are published every five minutes and can be monitored with graphs in the environment management console\. With enhanced health, the Elastic Beanstalk health agent reports metrics to Elastic Beanstalk every ten seconds\. Elastic Beanstalk uses the metrics provided by the health agent to determine the health status of each instance in the environment, and, combined with other [factors](#health-enhanced-factors), to determine the overall health of the environment\. 

 The overall health of the environment can be viewed in real\-time in the environment dashboard and is published to CloudWatch by Elastic Beanstalk every sixty seconds\. Detailed metrics reported by the health agent can be viewed in real time with the [eb health](health-enhanced-ebcli.md) command in the [EB CLI](eb-cli3.md)\.

For an additional charge, you can choose to publish individual instance and environment level metrics to CloudWatch every sixty seconds\. Metrics published to CloudWatch can then be used to create [monitoring graphs](environment-health-console.md#environment-health-console-customize) in the [environment management console](environments-console.md)\. 

 Enhanced health reporting only incurs a charge if you choose to publish enhanced health metrics to CloudWatch\. When you use enhanced health, you still get the basic health metrics published for free, even if you don't choose to publish enhanced health metrics\. 

See [Instance Metrics](health-enhanced-metrics.md) for details on the metrics reported by the health agent\. For details on publishing enhanced health metrics to CloudWatch, see [Publishing Amazon CloudWatch Custom Metrics for an Environment](health-enhanced-cloudwatch.md)\.

## Factors in Determining Instance and Environment Health<a name="health-enhanced-factors"></a>

In addition to the basic health reporting system checks, including [Elastic Load Balancing Health Checks](using-features.healthstatus.md#using-features.healthstatus.understanding) and [resource monitoring](using-features.healthstatus.md#monitoring-basic-additionalchecks), Elastic Beanstalk enhanced health reporting gathers additional data about the state of the instances in your environment, including operating system metrics, server logs, and the state of ongoing environment operations such as deployments and updates\. The Elastic Beanstalk health reporting service combines information from all available sources and analyzes it to determine the overall health of the environment\.

### Operations and Commands<a name="health-enhanced-factors-operations"></a>

When you perform an operation on your environment, such as deploying a new version of an application, Elastic Beanstalk makes several changes that cause the health status of the environment to change\.

For example, when you deploy a new version of an application to an environment that is running multiple instances, you might see messages similar to the following as you monitor the environment's health [with the EB CLI](health-enhanced-ebcli.md):

```
  id             status     cause
    Overall      Info       Command is executing on 3 out of 5 instances
  i-bb65c145     Pending    91 % of CPU is in use. 24 % in I/O wait
                            Performing application deployment (running for 31 seconds)
  i-ba65c144     Pending    Performing initialization (running for 12 seconds)
  i-f6a2d525     Ok         Application deployment completed 23 seconds ago and took 26 seconds
  i-e8a2d53b     Pending    94 % of CPU is in use. 52 % in I/O wait
                            Performing application deployment (running for 33 seconds)
  i-e81cca40     Ok
```

In this example, the overall status of the environment is `Ok` and the cause of this status is that the *Command is executing on 3 out of 5 instances*\. Three of the instances in the environment have the status *Pending*, indicating that an operation is in progress\.

When an operation completes, Elastic Beanstalk reports additional information about the operation\. For the example, Elastic Beanstalk displays the following information about an instance that has already been updated with the new version of the application:

```
i-f6a2d525     Ok         Application deployment completed 23 seconds ago and took 26 seconds
```

Instance health information also includes details about the most recent deployment to each instance in your environment\. Each instance reports a deployment ID and status\. The deployment ID is an integer that increases by one each time you deploy a new version of your application or change settings for on\-instance configuration options such as environment variables\. You can use the deployment information to identify instances that are running the wrong version of your application after a failed [rolling deployment](using-features.rolling-version-deploy.md)\.

In the cause column, Elastic Beanstalk includes informational messages about successful operations and other healthy states across multiple health checks, but they do not persist indefinitely\. Causes for unhealthy environment statuses persist until the environment returns to a healthy status\.

### Command Timeout<a name="health-enhanced-factors-timeout"></a>

Elastic Beanstalk applies a command timeout from the time an operation begins to allow an instance to transition into a healthy state\. This command timeout is set in your environment's update and deployment configuration \(in the [aws:elasticbeanstalk:command](command-options-general.md#command-options-general-elasticbeanstalkcommand) namespace\) and defaults to 10 minutes\. 

During rolling updates, Elastic Beanstalk applies a separate timeout to each batch in the operation\. This timeout is set as part of the environment's rolling update configuration \(in the [aws:autoscaling:updatepolicy:rollingupdate](command-options-general.md#command-options-general-autoscalingupdatepolicyrollingupdate) namespace\)\. If all instances in the batch are healthy within the command timeout, the operation continues to the next batch\. If not, the operation fails\.

**Note**  
If your application does not pass health checks with `Ok` status but is stable at a different level, you can set the `HealthCheckSuccessThreshold` option in the [`aws:elasticbeanstalk:command namespace`](command-options-general.md#command-options-general-elasticbeanstalkcommand) to change the level at which Elastic Beanstalk considers an instance to be healthy\.

For a web server environment to be considered healthy, each instance in the environment or batch must pass 12 consecutive health checks over the course of two minutes\. For worker tier, each instance must pass 18 health checks\. Prior to command timeout, Elastic Beanstalk does not lower an environment's health status when health checks fail\. As long as the instances in the environment and become healthy within the command timeout, the operation succeeds\.

### HTTP Requests<a name="health-enhanced-factors-requests"></a>

When no operation is in progress on an environment, the primary source of information about instance and environment health is the web server logs for each instance\. To determine the health of an instance and the overall health of the environment, Elastic Beanstalk considers the number of requests, the result of each request, and the speed at which each request was resolved\.

If you use Multicontainer Docker, which doesn't include a web server, or disable the web server \(nginx or Apache\) that is included in other Elastic Beanstalk platforms, additional configuration is required to get the [Elastic Beanstalk health agent](#health-enhanced-agent) logs in the format that it needs to relay health information to the Elastic Beanstalk service\. See [Enhanced Health Log Format](health-enhanced-serverlogs.md) for details\.

### Operating System Metrics<a name="health-enhanced-factors-healthcheck"></a>

Elastic Beanstalk monitors operating system metrics reported by the health agent to identify instances that are consistently low on system resources\.

See [Instance Metrics](health-enhanced-metrics.md) for details on the metrics reported by the health agent\.

## Health Check Rule Customization<a name="health-enhanced.rules"></a>

Elastic Beanstalk enhanced health reporting relies on a set of rules to determine the health of your environment\. Some of these rules might not be appropriate for your particular application\. A common case is an application that returns frequent HTTP 4xx errors by design\. Elastic Beanstalk, using one of its default rules, concludes that something is going wrong, and changes your environment health status from OK to Warning, Degraded, or Severe, depending on the error rate\. To handle this case correctly, Elastic Beanstalk allows you to configure this rule and ignore application HTTP 4xx errors\. For details, see [Configuring Enhanced Health Rules for an Environment](health-enhanced-rules.md)\.

## Enhanced Health Roles<a name="health-enhanced-roles"></a>

Enhanced health reporting requires two roles—a service role for Elastic Beanstalk and an instance profile for the environment\. The service role allows Elastic Beanstalk to interact with other AWS services on your behalf in order to gather information about the resources in your environment\. The instance profile allows the instances in your environment to write logs to Amazon S3\.

When you create an Elastic Beanstalk environment in the AWS Management Console, the console prompts you to create an instance profile and service role with appropriate permissions\. The EB CLI also assists you in creating these roles when you call eb create to create an environment\.

If you use the API, an SDK, or the AWS CLI to create environments, you must create these roles beforehand and specify them during environment creation to use enhanced health\. For instructions on creating appropriate roles for your environments, see [Service Roles, Instance Profiles, and User Policies](concepts-roles.md)\.

## Enhanced Health Events<a name="health-enhanced-events"></a>

The enhanced health system generates events when an environment transitions between states\. The following example shows events output by an environment transitioning between Info, OK and Severe states:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-events.png)

When transitioning to a worse state, Elastic Beanstalk includes a message indicating the cause in the event\.

Not all changes in status at an instance level will cause Elastic Beanstalk to emit an event\. To prevent false alarms, Elastic Beanstalk only generates a health related event if an issue persists across multiple checks\.

Real time environment level health information, including status, color and cause, is available in the [environment dashboard](environments-console.md#environments-dashboard) and the [EB CLI](eb-cli3.md)\. By attaching the EB CLI to your environment and running the [eb health](health-enhanced-ebcli.md) command, you can also view real time statuses from each of the *instances *in your environment\.

## Enhanced Health Reporting Behavior During Updates, Deployments, and Scaling<a name="health-enhanced-effects"></a>

Enabling enhanced health reporting can affect how your environment behaves during configuration updates and deployments\. Elastic Beanstalk won't complete a batch of updates until all of the instances pass health checks consistently, and since enhanced health reporting applies a higher standard for health and monitors more factors, instances that pass basic health reporting's [ELB health check](using-features.healthstatus.md#using-features.healthstatus.understanding) won't necessarily pass muster with enhanced health reporting\. See the topics on [rolling configuration updates](using-features.rollingupdates.md) and [rolling deployments](using-features.rolling-version-deploy.md) for details on how health checks affect the update process\.

Enhanced health reporting can also highlight the need to set a proper [health check URL](environments-cfg-clb.md#using-features.managing.elb.healthchecks) for Elastic Load Balancing\. When your environment scales up to meet demand, new instances will start taking requests as soon as they pass enough ELB health checks\. If a health check URL is not configured, this can be as little as 20 seconds after a new instance is able to accept a TCP connection\.

If your application hasn't finished starting up by the time the load balancer declares it healthy enough to receive traffic, you will see a flood of failed requests, and your environment will start to fail health checks\. A health check URL that hits a path served by your application can prevent this issue; ELB health checks won't pass until a GET request to the health check URL returns a 200 status code\.