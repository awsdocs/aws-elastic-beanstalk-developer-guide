# Example: Using custom amazon CloudWatch metrics<a name="customize-containers-cw"></a>

Amazon CloudWatch is a web service that enables you to monitor, manage, and publish various metrics, as well as configure alarm actions based on data from metrics\. You can define custom metrics for your own use, and Elastic Beanstalk will push those metrics to Amazon CloudWatch\. Once Amazon CloudWatch contains your custom metrics, you can view those in the Amazon CloudWatch console\.

The Amazon CloudWatch Monitoring Scripts for Linux are available to demonstrate how to produce and consume Amazon CloudWatch custom metrics\. The scripts comprise a fully functional example that reports memory, swap, and disk space utilization metrics for an Amazon Elastic Compute Cloud \(Amazon EC2\) Linux instance\. For more information about the Amazon CloudWatch Monitoring Scripts, go to [Amazon CloudWatch Monitoring Scripts for Linux](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/mon-scripts.html) in the *Amazon CloudWatch Developer Guide*\.

**Note**  
Elastic Beanstalk [Enhanced Health Reporting](health-enhanced.md) has native support for publishing a wide range of instance and environment metrics to CloudWatch\. See [Publishing Amazon CloudWatch custom metrics for an environment](health-enhanced-cloudwatch.md) for details\.

**Topics**
+ [\.Ebextensions configuration file](#customize-containers-cw-update-roles)
+ [Permissions](#customize-containers-cw-policy)
+ [Viewing metrics in the CloudWatch console](#customize-containers-cw-console)

## \.Ebextensions configuration file<a name="customize-containers-cw-update-roles"></a>

This example uses commands and option settings in an \.ebextensions configuration file to download, install, and run monitoring scripts provided by Amazon CloudWatch\.

To use this sample, save it to a file named `cloudwatch.config` in a directory named `.ebextensions` at the top level of your project directory, then deploy your application using the Elastic Beanstalk console \(include the \.ebextensions directory in your [source bundle](applications-sourcebundle.md)\) or the [EB CLI](eb-cli3.md)\.

For more information about configuration files, see [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)\.

**\.ebextensions/cloudwatch\.config**

```
packages:
  yum:
    perl-DateTime: []
    perl-Sys-Syslog: []
    perl-LWP-Protocol-https: []
    perl-Switch: []
    perl-URI: []
    perl-Bundle-LWP: []

sources: 
  /opt/cloudwatch: https://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.1.zip
  
container_commands:
  01-setupcron:
    command: |
      echo '*/5 * * * * root perl /opt/cloudwatch/aws-scripts-mon/mon-put-instance-data.pl `{"Fn::GetOptionSetting" : { "OptionName" : "CloudWatchMetrics", "DefaultValue" : "--mem-util --disk-space-util --disk-path=/" }}` >> /var/log/cwpump.log 2>&1' > /etc/cron.d/cwpump
  02-changeperm:
    command: chmod 644 /etc/cron.d/cwpump
  03-changeperm:
    command: chmod u+x /opt/cloudwatch/aws-scripts-mon/mon-put-instance-data.pl

option_settings:
  "aws:autoscaling:launchconfiguration" :
    IamInstanceProfile : "aws-elasticbeanstalk-ec2-role"
  "aws:elasticbeanstalk:customoption" :
    CloudWatchMetrics : "--mem-util --mem-used --mem-avail --disk-space-util --disk-space-used --disk-space-avail --disk-path=/ --auto-scaling"
```

After you verify the configuration file works, you can conserve disk usage by changing the command redirect from a log file \(`>> /var/log/cwpump.log 2>&1`\) to `/dev/null` \(`> /dev/null`\)\. 

## Permissions<a name="customize-containers-cw-policy"></a>

In order to publish custom Amazon CloudWatch metrics, the instances in your environment need permission to use CloudWatch\. You can grant permissions to your environment's instances by adding them to the environment's [instance profile](concepts-roles-instance.md)\. You can add permissions to the instance profile before or after deploying your application\.

**To grant permissions to publish CloudWatch metrics**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose your environment's instance profile role\. By default, when you create an environment with the Elastic Beanstalk console or [EB CLI](eb-cli3.md), this is `aws-elasticbeanstalk-ec2-role`\.

1. Choose the **Permissions** tab\.

1. Under **Inline Policies**, in the **Permissions** section, choose **Create Role Policy**\.

1. Choose **Custom Policy**, and then choose **Select**\.

1. Complete the following fields, and then choose **Apply Policy**:  
**Policy Name**  
The name of the policy\.  
**Policy Document**  
Copy and paste the following text into the policy document:  

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Action": [
           "cloudwatch:PutMetricData",
           "ec2:DescribeTags"
         ],
         "Effect": "Allow",
         "Resource": [
           "*"
         ]
       }
     ]
   }
   ```

   For more information about managing policies, see [Working with Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) in the *IAM User Guide*\.

## Viewing metrics in the CloudWatch console<a name="customize-containers-cw-console"></a>

After deploying the CloudWatch configuration file to your environment, check the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/home) to view your metrics\. Custom metrics will have the prefix **Linux System**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-container-cw.png)