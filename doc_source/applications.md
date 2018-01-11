# Managing and Configuring AWS Elastic Beanstalk Applications<a name="applications"></a>

The first step in using Elastic Beanstalk is to create an application, which represents your web application in AWS\. In Elastic Beanstalk an application serves as a container for the environments that run your web app, and versions of your web app's source code, saved configurations, logs and other artifacts that you create while using Elastic Beanstalk\.

**To create a new application**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **Create New Application**\.

1. Enter the name of the application and, optionally, a description\. Then click **Next**\.

After creating a new application, the console prompts you to create an environment for it\. For detailed information about all of the options available, see \.

If you have no further need for an application, you can delete it\.

**Warning**  
Deleting an application terminates all associated environments and deletes all application versions and saved configurations that belong to the application\.

**To delete an application**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **Actions** next to the application that you want to delete\.

1. Choose **Delete Application**


+ [The AWS Elastic Beanstalk Application Management Console](applications-console.md)
+ [Managing Application Versions](applications-versions.md)
+ [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)
+ [Create an Application Source Bundle](applications-sourcebundle.md)