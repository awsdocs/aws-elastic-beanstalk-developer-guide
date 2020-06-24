# Elastic Beanstalk application management console<a name="applications-console"></a>

You can use the AWS Elastic Beanstalk console to manage applications, application versions, and saved configurations\.

**To access the application management console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose your application's name from the list\.
**Note**  
If you have many applications, use the search bar to filter the application list\.

   The application overview page shows a list with an overview of all environments associated with the application\.  
![\[The application overview page with a list of application environments on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-mgmt-environments.png)

1. You have a few ways to continue:

   1. Choose the **Actions** drop\-down menu, and then choose one of the application management actions\. To launch an environment in this application, you can directly choose **Create a new environment**\. For details, see [Creating an Elastic Beanstalk environment](using-features.environments.md)\.

   1. Choose an environment name to go to the [environment management console](environments-console.md) for that environment, where you can configure, monitor, or manage the environment\.

   1. Choose **Application versions** following the application name in the navigation pane to view and manage the application versions for your application\.

      An application version is an uploaded version of your application code\. You can upload new versions, deploy an existing version to any of the application's environments, or delete old versions\. For more information, see [Managing application versions](applications-versions.md)\.

   1. Choose **Saved configurations** following the application name in the navigation pane to view and manage configurations saved from running environments\.

      A saved configuration is a collection of settings that you can use to restore an environment's settings to a previous state, or to create an environment with the same settings\. For more information see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\.