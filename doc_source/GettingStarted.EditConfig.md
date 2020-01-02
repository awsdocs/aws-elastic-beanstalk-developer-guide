# Step 4: Configure Your Environment<a name="GettingStarted.EditConfig"></a>

You can configure your environment to better suit your application\. For example, if you have a compute\-intensive application, you can change the type of Amazon Elastic Compute Cloud \(Amazon EC2\) instance that is running your application\. To apply configuration changes, Elastic Beanstalk performs an environment update\.

Some configuration changes are simple and happen quickly\. Some changes require deleting and recreating AWS resources, which can take several minutes\. When you change configuration settings, Elastic Beanstalk warns you about potential application downtime\. 

## Make a Configuration Change<a name="GettingStarted.EditConfig.Edit"></a>

In this example of a configuration change, you edit your environment's capacity settings\. You configure a load balanced, automatically scaling environment that has between two and four Amazon EC2 instances in its Auto Scaling group, and then you verify that the change occurred\. Elastic Beanstalk creates an additional Amazon EC2 instance, adding to the single instance that it created initially\. Then, Elastic Beanstalk associates both instances with the environment's load balancer\. As a result, your application's responsiveness is improved and its availability is increased\.

**To change your environment's capacity**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Capacity** configuration category, choose **Modify**\.  
![\[Capacity configuration category\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-capacity.png)

1. In the **Auto Scaling group** section, change **Environment type** to **Load balanced**\.

1. In the **Instances** row, change **Max** to **4**, and then change **Min** to **2**\.

1. On the **Modify capacity** page, choose **Save**\.

1. On the **Configuration overview** page, choose **Apply**\.

1. A warning tells you that this update replaces all of your current instances\. Choose **Confirm**\.

1. In the navigation pane, choose **Events**\.

   The environment update can take a few minutes\. To find out that it's complete, look for the event **Successfully deployed new configuration to environment** in the event list\. This confirms that the Auto Scaling minimum instance count has been set to 2\. Elastic Beanstalk automatically launches the second instance\. 

## Verify the Configuration Change<a name="GettingStarted.EditConfig.Verify"></a>

When the environment update is complete and the environment is ready, verify your change\.

**To verify the increased capacity**

1. In the navigation pane, choose **Health**\.

1. Look at the **Enhanced Health Overview** page\.

   You can see that the **Total** number of instances is **2**\. You can also see that two Amazon EC2 instances are listed under the **Overall** line\. Your environment capacity has increased to two instances\.  
![\[Enhanced Health Overview page on the Elastic Beanstalk Console showing two Amazon EC2 instances\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-health.png)