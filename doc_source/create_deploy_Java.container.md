# Configuring Java containers using AWS Toolkit for Eclipse<a name="create_deploy_Java.container"></a>

The **Container/JVM Options** panel lets you fine\-tune the behavior of the Java Virtual Machine on your Amazon EC2 instances and enable or disable Amazon S3 log rotation\. You can use the AWS Toolkit for Eclipse to configure your container information\. For more information on the options available for Tomcat environments, see [Configuring your Tomcat environment](java-tomcat-platform.md#java-tomcat-options)\.

**Note**  
You can modify your configuration settings with zero downtime by swapping the CNAME for your environments\. For more information, see [Blue/Green deployments with Elastic Beanstalk](using-features.CNAMESwap.md)\.

**To access the Container/JVM options panel for your Elastic Beanstalk application**

1. If Eclipse isn't displaying the **AWS Explorer** view, in the menu choose **Window**, **Show View**, **AWS Explorer**\. Expand the Elastic Beanstalk node and your application node\. 

1. In the **AWS Explorer**, double\-click your Elastic Beanstalk environment\.

1. At the bottom of the pane, click the **Configuration** tab\.

1. Under **Container**, you can configure container options\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-container-options.png)

## Remote debugging<a name="create_deploy_Java.container.eclipse.remotedebug"></a>

To test your application remotely, you can run your application in debug mode\.

**To enable remote debugging**

1. Select **Enable remote debugging**\. 

1. For **Remote debugging port**, specify the port number to use for remote debugging\. 

   The **Additional Tomcat JVM command line options** setting is filled automatically\. 

**To start remote debugging**

1. In the AWS Toolkit for Eclipse menu, choose **Window**, **Show View**, **Other**\. 

1. Expand the **Server** folder, and then choose **Servers**\. Choose **OK**\.

1. In the **Servers** pane, right\-click the server your application is running on, and then click **Restart in Debug**\.