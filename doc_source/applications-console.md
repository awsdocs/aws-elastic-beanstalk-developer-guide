# The AWS Elastic Beanstalk Application Management Console<a name="applications-console"></a>

You can use the AWS Management Console to manage applications, application versions, and saved configurations\.

**To access the application management console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. The console displays a list of all environments running in the current region, sorted by application\. Choose an application name to view the management console for that application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-choose-application.png)
**Note**  
If you have a large number of applications, use the search bar in the upper right corner to filter the list of applications\.

   The **Environments** page shows an overview of all environments associated with the application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-mgmt-environments.png)

1. Choose an environment by name to go to the [environment management console](environments-console.md) for that environment to configure, monitor, or manage it\.

   Choose **Application Versions** to view a list of application versions for your application:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-mgmt-versions.png)

   Here you can upload new versions, deploy an existing version to any of the application's environments, or delete old versions\. See [Managing Application Versions](applications-versions.md) for more information about the options on this page\.

1. Choose **Saved Configurations** to view a list of configurations saved from running environments\. A saved configuration is a collection of settings that you can use to restore an environment's settings to a previous state or create a new environment with the same settings\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-mgmt-savedconfigs.png)

   See [Using Elastic Beanstalk Saved Configurations](environment-configuration-savedconfig.md) for more information\.