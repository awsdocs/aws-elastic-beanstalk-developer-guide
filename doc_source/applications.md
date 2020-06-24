# Managing and configuring Elastic Beanstalk applications<a name="applications"></a>

The first step in using AWS Elastic Beanstalk is to create an application, which represents your web application in AWS\. In Elastic Beanstalk an application serves as a container for the environments that run your web app and for versions of your web app's source code, saved configurations, logs, and other artifacts that you create while using Elastic Beanstalk\.

**To create an application**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose **Create a new application**\.

1. Use the on\-screen form to provide an application name\.

1. Optionally, provide a description, and add tag keys and values\.

1. Choose **Create**\.

![\[Create new application dialog box in the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-create-dialog.png)

After creating the application, the console prompts you to create an environment for it\. For detailed information about all of the options available, see [Creating an Elastic Beanstalk environment](using-features.environments.md)\.

If you no longer need an application, you can delete it\.

**Warning**  
Deleting an application terminates all associated environments and deletes all application versions and saved configurations that belong to the application\.

**To delete an application**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then select your application on the list\.

1. Choose **Actions**, and then choose **Delete application**\.

**Topics**
+ [Elastic Beanstalk application management console](applications-console.md)
+ [Managing application versions](applications-versions.md)
+ [Create an application source bundle](applications-sourcebundle.md)
+ [Tagging Elastic Beanstalk application resources](applications-tagging-resources.md)