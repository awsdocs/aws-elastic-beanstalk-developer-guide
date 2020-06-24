# Configuring enhanced health rules for an environment<a name="health-enhanced-rules"></a>

AWS Elastic Beanstalk enhanced health reporting relies on a set of rules to determine the health of your environment\. Some of these rules might not be appropriate for your particular application\. The following are some common examples:
+ You use client\-side test tools\. In this case, frequent HTTP client \(4xx\) errors are expected\.
+ You use [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/) in conjunction with your environment's Application Load Balancer to block unwanted incoming traffic\. In this case, Application Load Balancer returns HTTP 403 for each rejected incoming message\.

By default, Elastic Beanstalk includes all application HTTP 4xx errors when determining the environment's health\. It changes your environment health status from **OK** to **Warning**, **Degraded**, or **Severe**, depending on the error rate\. To correctly handle cases such as the examples we mentioned, Elastic Beanstalk enables you to configure some enhanced health rules\. You can choose to ignore application HTTP 4xx errors on the environment's instances, or to ignore HTTP 4xx errors returned by the environment's load balancer\. This topic describes how to make these configuration changes\.

**Note**  
Currently, these are the only available enhanced heath rule customizations\. You can't configure enhanced health to ignore other HTTP errors in addition to 4xx\.

## Configuring enhanced health rules using the Elastic Beanstalk console<a name="health-enhanced-rules.console"></a>

You can use the Elastic Beanstalk console to configure enhanced health rules in your environment\.

**To configure HTTP 4xx status code checking using the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Monitoring** configuration category, choose **Edit**\.

1. Under **Health monitoring rule customization**, enable or disable the desired **Ignore** options\.  
![\[Health monitoring rule customization section on the monitoring configuration page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-rule-customization.png)

1. Choose **Apply**\.

## Configuring enhanced health rules using the EB CLI<a name="health-enhanced-rules.ebcli"></a>

You can use the EB CLI to configure enhanced health rules by saving your environment's configuration locally, adding an entry that configures enhanced health rules, and then uploading the configuration to Elastic Beanstalk\. You can apply the saved configuration to an environment during or after creation\.

**To configure HTTP 4xx status code checking using the EB CLI and saved configurations**

1. Initialize your project folder with [eb init](eb-cli3-configuration.md)\.

1. Create an environment by running the [eb create](eb-cli3-getting-started.md) command\.

1. Save a configuration template locally by running the eb config save command\. The following example uses the `--cfg` option to specify the name of the configuration\.

   ```
   $ eb config save --cfg 01-base-state
   Configuration saved at: ~/project/.elasticbeanstalk/saved_configs/01-base-state.cfg.yml
   ```

1. Open the saved configuration file in a text editor\.

1. Under `OptionSettings` > `aws:elasticbeanstalk:healthreporting:system:`, add a `ConfigDocument` key to list each enhanced health rule to configure\. The following `ConfigDocument` disables the checking of application HTTP 4xx status codes, while keeping the checking of load balancer HTTP 4xx code enabled\.

   ```
   OptionSettings:
     ...
     aws:elasticbeanstalk:healthreporting:system:
       ConfigDocument:
         Rules:
           Environment:
             Application:
               ApplicationRequests4xx:
                 Enabled: false
             ELB:
               ELBRequests4xx:
                 Enabled: true
         Version: 1
       SystemType: enhanced
   ...
   ```
**Note**  
You can combine `Rules` and `CloudWatchMetrics` in the same `ConfigDocument` option setting\. `CloudWatchMetrics` are described in [Publishing Amazon CloudWatch custom metrics for an environment](health-enhanced-cloudwatch.md)\.  
If you previously enabled `CloudWatchMetrics`, the configuration file that you retrieve using the eb config save command already has a `ConfigDocument` key with a `CloudWatchMetrics` section\. *Do not delete it*—add a `Rules` section into the same `ConfigDocument` option value\.

1. Save the configuration file and close the text editor\. In this example, the updated configuration file is saved with a name \(`02-cloudwatch-enabled.cfg.yml`\) that's different from the downloaded configuration file\. This creates a separate saved configuration when the file is uploaded\. You can use the same name as the downloaded file to overwrite the existing configuration without creating a new one\.

1. Use the eb config put command to upload the updated configuration file to Elastic Beanstalk\.

   ```
   $ eb config put 02-cloudwatch-enabled
   ```

   When using the eb config `get` and `put` commands with saved configurations, don't include the file name extension\.

1. Apply the saved configuration to your running environment\.

   ```
   $ eb config --cfg 02-cloudwatch-enabled
   ```

   The `--cfg` option specifies a named configuration file that is applied to the environment\. You can save the configuration file locally or in Elastic Beanstalk\. If a configuration file with the specified name exists in both locations, the EB CLI uses the local file\.

## Configuring enhanced health rules using a config document<a name="health-enhanced-rules.configdocument"></a>

The configuration \(config\) document for enhanced health rules is a JSON document that lists the rules to configure\. 

The following example shows a config document that disables the checking of application HTTP 4xx status codes and enables the checking of load balancer HTTP 4xx status codes\.

```
{
  "Rules": {
    "Environment": {
      "Application": {
        "ApplicationRequests4xx": {
          "Enabled": false
        }
      },
      "ELB": {
        "ELBRequests4xx": {
          "Enabled": true
        }
      }
    }
  },
  "Version": 1
}
```

For the AWS CLI, you pass the document as a value for the `Value` key in an option settings argument, which itself is a JSON object\. In this case, you must escape quotation marks in the embedded document\. The following command checks if the configuration settings are valid\.

```
$ aws elasticbeanstalk validate-configuration-settings --application-name my-app --environment-name my-env --option-settings '[
    {
        "Namespace": "aws:elasticbeanstalk:healthreporting:system",
        "OptionName": "ConfigDocument",
        "Value": "{\"Rules\": { \"Environment\": { \"Application\": { \"ApplicationRequests4xx\": { \"Enabled\": false } }, \"ELB\": { \"ELBRequests4xx\": {\"Enabled\": true } } } }, \"Version\": 1 }"
    }
]'
```

For an `.ebextensions` configuration file in YAML, you can provide the JSON document as is\.

```
  option_settings:
    - namespace: aws:elasticbeanstalk:healthreporting:system
      option_name: ConfigDocument
      value: {
  "Rules": {
    "Environment": {
      "Application": {
        "ApplicationRequests4xx": {
          "Enabled": false
        }
      },
      "ELB": {
        "ELBRequests4xx": {
          "Enabled": true
        }
      }
    }
  },
  "Version": 1
}
```