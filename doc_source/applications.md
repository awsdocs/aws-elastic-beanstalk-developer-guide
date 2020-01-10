# Managing and Configuring Elastic Beanstalk Applications<a name="applications"></a>

The first step in using AWS Elastic Beanstalk is to create an application, which represents your web application in AWS\. In Elastic Beanstalk an application serves as a container for the environments that run your web app, and versions of your web app's source code, saved configurations, logs, and other artifacts that you create while using Elastic Beanstalk\.

**To create an application**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **Create New Application**\.

1. Use the on\-screen form to provide the required details\.
   + Optionally, provide a description, and add tag keys and values\.

1. Choose **Create**\.

After creating the application, the console prompts you to create an environment for it\. For detailed information about all of the options available, see [Creating an Elastic Beanstalk Environment](using-features.environments.md)\.

If you no longer need an application, you can delete it\.

**Warning**  
Deleting an application terminates all associated environments and deletes all application versions and saved configurations that belong to the application\.

**To delete an application**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **Actions** next to the application to delete\.

1. Choose **Delete Application**\.

**Topics**
+ [Elastic Beanstalk Application Management Console](applications-console.md)
+ [Managing Application Versions](applications-versions.md)
+ [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)
+ [Create an Application Source Bundle](applications-sourcebundle.md)
+ [Tagging Elastic Beanstalk Application Resources](applications-tagging-resources.md)