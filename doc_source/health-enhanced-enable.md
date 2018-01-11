# Enabling AWS Elastic Beanstalk Enhanced Health Reporting<a name="health-enhanced-enable"></a>

New environments created with the latest platform versions include the AWS Elastic Beanstalk health agent, which supports enhanced health reporting\. If you create your environment in the AWS Management Console or with the EB CLI, enhanced health is enabled by default\. You can also set your health reporting preference in your application's source code using configuration files\.

Enhanced health reporting requires an instance profile and service role with the standard set of permissions\. When you create an environment in the Elastic Beanstalk Management Console, Elastic Beanstalk creates the required roles automatically\. See  for instructions on creating your first environment\.


+ [Enabling Enhanced Health Reporting with the AWS Management Console](#w3ab1c25c11c45b9)
+ [Enabling Enhanced Health Reporting with the EB CLI](#w3ab1c25c11c45c11)
+ [Enabling Enhanced Health Reporting with a Configuration File](#w3ab1c25c11c45c13)

## Enabling Enhanced Health Reporting with the AWS Management Console<a name="w3ab1c25c11c45b9"></a>

**To enable enhanced health reporting in a running environment with the AWS Management Console**

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Health** panel under **Web Tier**, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\.

1. Under **Health Reporting**, for **System type**, choose **Enhanced**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-dashboard-option.png)
**Note**  
The options for enhanced health reporting do not appear if you are using an unsupported platform or version\.

1. Choose **Apply**\.

The Elastic Beanstalk console defaults to enhanced health reporting when you create a new environment with a version 2 platform configuration\. You can disable enhanced health reporting by changing the health reporting option during environment creation\.

**To disable enhanced health reporting when creating an environment using the AWS Management Console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Create an application or select an existing one\.

1. Create an environment\. On the **Configuration Details** page, choose **Basic** for the health reporting type under **Health Reporting**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-console-option.png)

## Enabling Enhanced Health Reporting with the EB CLI<a name="w3ab1c25c11c45c11"></a>

When you create a new environment with the `eb create` command, the EB CLI enables enhanced health reporting by default and applies the default instance profile and service role\.

You can specify a different service role by name with the `--service-role` option\.

If you have an environment running with basic health reporting on a version 2 platform configuration and want to switch to enhanced health, follow these steps:

**To enable enhanced health on a running environment using the EB CLI**

1. Use the `eb config` command to open the configuration file in the default text editor:

   ```
   ~/project$ eb config
   ```

1. Locate the `aws:elasticbeanstalk:environment` namespace in the settings section\. Ensure that the value of `ServiceRole` is not null and that it matches the name of your service role\.

   ```
     aws:elasticbeanstalk:environment:
       EnvironmentType: LoadBalanced
       ServiceRole: aws-elasticbeanstalk-service-role
   ```

1. Under the `aws:elasticbeanstalk:healthreporting:system:` namespace, change the value of `SystemType` to **enhanced**\.

   ```
     aws:elasticbeanstalk:healthreporting:system:
       SystemType: enhanced
   ```

1. Save the configuration file and close the text editor\.

1. The EB CLI starts an environment update to apply your configuration changes\. Wait for the operation to complete or press **Ctrl\-C** to exit safely\.

   ```
   ~/project$ eb config
   Printing Status:
   INFO: Environment update is starting.
   INFO: Health reporting type changed to ENHANCED.
   INFO: Updating environment no-role-test's configuration settings.
   ```

## Enabling Enhanced Health Reporting with a Configuration File<a name="w3ab1c25c11c45c13"></a>

You can enable enhanced health reporting by including a configuration file in your source bundle\. The following example shows a configuration file that enables enhanced health reporting and assigns the default service and instance profile to the environment:

**Example \.ebextensions/enhanced\-health\.config**  

```
option_settings:
  aws:elasticbeanstalk:healthreporting:system:
    SystemType: enhanced
  aws:autoscaling:launchconfiguration:
    IamInstanceProfile: aws-elasticbeanstalk-ec2-role
  aws:elasticbeanstalk:environment:
    ServiceRole: aws-elasticbeanstalk-service-role
```

If you created your own instance profile or service role, replace the highlighted text with the names of those roles\.