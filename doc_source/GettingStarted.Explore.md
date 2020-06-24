# Step 2: Explore your environment<a name="GettingStarted.Explore"></a>

To see an overview of your Elastic Beanstalk application's environment, use the environment page in the Elastic Beanstalk console\.

**To view the environment overview**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

The environment overview pane shows top level information about your environment\. This includes its name, its URL, its current health status, the name of the currently deployed application version, and the platform version that the application is running on\. Below the overview pane you can see the five most recent environment events\.

To learn more about environment tiers, platforms, application versions, and other Elastic Beanstalk concepts, see [Elastic Beanstalk concepts](concepts.md)\.

![\[Environment overview.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-dashboard.png)

While Elastic Beanstalk creates your AWS resources and launches your application, the environment is in a `Pending` state\. Status messages about launch events are continuously added to the overview\.

The environment's **URL** is located at the top of the overview, below the environment name\. This is the URL of the web application that the environment is running\. Choose this URL to get to the example application's *Congratulations* page\.

The navigation page on the left side of the console links to other pages that contain more detailed information about your environment and provide access to additional features:
+ **Configuration** – Shows the resources provisioned for this environment, such as the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that host your application\. You can configure some of the provisioned resources on this page\.
+ **Health** – Shows the status of and detailed health information about the Amazon EC2 instances running your application\.
+ **Monitoring** – Shows statistics for the environment, such as average latency and CPU utilization\. You can use this page to create alarms for the metrics that you are monitoring\.
+ **Events** – Shows information or error messages from the Elastic Beanstalk service and from other services whose resources this environment uses\.
+ **Tags** – Shows environment tags and allows you to manage them\. Tags are key\-value pairs that are applied to your environment\.