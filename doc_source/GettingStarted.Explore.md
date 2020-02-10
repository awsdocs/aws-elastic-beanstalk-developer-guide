# Step 2: Explore your environment<a name="GettingStarted.Explore"></a>

To see an overview of your Elastic Beanstalk application's environment, use the environment dashboard in the Elastic Beanstalk console\.

**To view the environment dashboard**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **GettingStartedApp\-env**\.  
![\[Choosing an environment on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-chooseenvironment.png)

The environment dashboard shows top level information about your environment\. This includes its URL, its current health status, the name of the currently deployed application version, its five most recent events, and the platform version that the application is running on\.

To learn more about environment tiers, platforms, application versions, and other Elastic Beanstalk concepts, see [Elastic Beanstalk concepts](concepts.md)\.

![\[Environment dashboard.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-dashboard.png)

While Elastic Beanstalk creates your AWS resources and launches your application, the environment is in a `Pending` state\. Status messages about launch events are continuously added to the dashboard\.

The environment's **URL** is located in the upper\-right corner of the dashboard, next to the **Actions** menu\. This is the URL of the web application that the environment is running\. Choose this URL to get to the example application's *Congratulations* page\.

The navigation page on the left side of the console links to other pages that contain more detailed information about your environment and provide access to additional features:
+ **Configuration** – Shows the resources provisioned for this environment, such as the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that host your application\. You can configure some of the provisioned resources on this page\.
+ **Health** – Shows the status of and detailed health information about the Amazon EC2 instances running your application\.
+ **Monitoring** – Shows statistics for the environment, such as average latency and CPU utilization\. You can use this page to create alarms for the metrics that you are monitoring\.
+ **Events** – Shows information or error messages from the Elastic Beanstalk service and from other services whose resources this environment uses\.
+ **Tags** – Shows environment tags and allows you to manage them\. Tags are key\-value pairs that are applied to your environment\.