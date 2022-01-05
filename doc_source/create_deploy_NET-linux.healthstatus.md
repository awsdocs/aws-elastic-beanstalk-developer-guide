# Monitoring application health<a name="create_deploy_NET-linux.healthstatus"></a>

It is important to know that your production website is available and responding to requests\. Elastic Beanstalk provides features to help you monitor your application's responsiveness\. It monitors statistics about your application and alerts you when thresholds are exceeded\.

For information about the health monitoring provided by Elastic Beanstalk, see [Basic health reporting](using-features.healthstatus.md)\.

You can access operational information about your application by using either the AWSToolkit for Visual Studio or the AWS Management Console\.

The toolkit displays your environment's status and application health in the **Status** field\.

![\[Elastic Beanstalk health status\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-application-tab-status.png)

**To monitor application health**

1. In the AWS Toolkit for Visual Studio, in **AWS Explorer**, expand the Elastic Beanstalk node, and then expand your application node\. 

1. Open the context \(right\-click\) menu for your application environment and select **View Status**\.

1. On your application environment tab, select **Monitoring**\.

   The **Monitoring** panel includes a set of graphs showing resource usage for your particular application environment\.  
![\[Elastic Beanstalk monitoring panel\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-monitoring.png)
**Note**  
By default, the time range is set to the last hour\. To modify this setting, in the **Time Range** list, select a different time range\.

You can use the AWS Toolkit for Visual Studio or the AWS Management Console to view events associated with your application\.

**To view application events**

1. In the AWS Toolkit for Visual Studio, in **AWS Explorer**, expand the Elastic Beanstalk node and your application node\. 

1. Open the context \(right\-click\) menu for your application environment and select **View Status**\.

1. In your application environment tab, select **Events**\.  
![\[Elastic Beanstalk events panel\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-events.png)