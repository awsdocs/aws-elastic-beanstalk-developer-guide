# Publishing Amazon CloudWatch custom metrics for an environment<a name="health-enhanced-cloudwatch"></a>

You can publish the data gathered by AWS Elastic Beanstalk enhanced health reporting to Amazon CloudWatch as custom metrics\. Publishing metrics to CloudWatch lets you monitor changes in application performance over time and identify potential issues by tracking how resource usage and request latency scale with load\.

By publishing metrics to CloudWatch, you also make them available for use with [monitoring graphs](environment-health-console.md#environment-health-console-graphs) and [alarms](using-features.alarms.md)\. One free metric, *EnvironmentHealth*, is enabled automatically when you use enhanced health reporting\. Custom metrics other than *EnvironmentHealth* incur standard [CloudWatch charges](https://aws.amazon.com/cloudwatch/pricing/)\. 

To publish CloudWatch custom metrics for an environment, you must first enable enhanced health reporting on the environment\. See [Enabling Elastic Beanstalk enhanced health reporting](health-enhanced-enable.md) for instructions\.

**Topics**
+ [Enhanced health reporting metrics](#health-enhanced-cloudwatch-metrics)
+ [Configuring CloudWatch metrics using the Elastic Beanstalk console](#health-enhanced-cloudwatch-console)
+ [Configuring CloudWatch custom metrics using the EB CLI](#health-enhanced-cloudwatch-ebcli)
+ [Providing custom metric config documents](#health-enhanced-cloudwatch-configdocument)

## Enhanced health reporting metrics<a name="health-enhanced-cloudwatch-metrics"></a>

When you enable enhanced health reporting in your environment, the enhanced health reporting system automatically publishes one [CloudWatch custom metric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/publishingMetrics.html), *EnvironmentHealth*\. To publish additional metrics to CloudWatch, configure your environment with those metrics by using the [Elastic Beanstalk console](#health-enhanced-cloudwatch-console), [EB CLI](#health-enhanced-cloudwatch-ebcli), or [\.ebextensions](command-options.md)\.

You can publish the following enhanced health metrics from your environment to CloudWatch\.Available metrics—all platforms

`EnvironmentHealth`  
*Environment only\.* This is the only CloudWatch metric that the enhanced health reporting system publishes, unless you configure additional metrics\. Environment health is represented by one of seven [statuses](health-enhanced-status.md)\. In the CloudWatch console, these statuses map to the following values:  
+ 0 – OK
+ 1 – Info
+ 5 – Unknown
+ 10 – No data
+ 15 – Warning
+ 20 – Degraded
+ 25 – Severe

`InstancesSevere``InstancesDegraded``InstancesWarning``InstancesInfo``InstancesOk``InstancesPending``InstancesUnknown``InstancesNoData`  
*Environment only\.* These metrics indicate the number of instances in the environment with each health status\. `InstancesNoData` indicates the number of instances for which no data is being received\.

`ApplicationRequestsTotal``ApplicationRequests5xx``ApplicationRequests4xx``ApplicationRequests3xx``ApplicationRequests2xx`  
*Instance and environment\.* Indicates the total number of requests completed by the instance or environment, and the number of requests that completed with each status code category\.

`ApplicationLatencyP10``ApplicationLatencyP50``ApplicationLatencyP75``ApplicationLatencyP85``ApplicationLatencyP90``ApplicationLatencyP95``ApplicationLatencyP99``ApplicationLatencyP99.9`  
*Instance and environment\.* Indicates the average amount of time, in seconds, it takes to complete the fastest *x* percent of requests\.

`InstanceHealth`  
*Instance only\.* Indicates the current health status of the instance\. Instance health is represented by one of seven [statuses](health-enhanced-status.md)\. In the CloudWatch console, these statuses map to the following values:  
+ 0 – OK
+ 1 – Info
+ 5 – Unknown
+ 10 – No data
+ 15 – Warning
+ 20 – Degraded
+ 25 – SevereAvailable metrics—Linux

`CPUIrq``CPUIdle``CPUUser``CPUSystem``CPUSoftirq``CPUIowait``CPUNice`  
*Instance only\.* Indicates the percentage of time that the CPU has spent in each state over the last minute\.

`LoadAverage1min`  
*Instance only\.* The average CPU load of the instance over the last minute\.

`RootFilesystemUtil`  
*Instance only\.* Indicates the percentage of disk space that's in use\.Available metrics—Windows

`CPUIdle``CPUUser``CPUPriveleged`  
Instance only\. Indicates the percentage of time that the CPU has spent in each state over the last minute\.

## Configuring CloudWatch metrics using the Elastic Beanstalk console<a name="health-enhanced-cloudwatch-console"></a>

You can use the Elastic Beanstalk console to configure your environment to publish enhanced health reporting metrics to CloudWatch and make them available for use with monitoring graphs and alarms\.

**To configure CloudWatch custom metrics in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Monitoring** configuration category, choose **Edit**\.

1. Under **Health reporting**, select the instance and environment metrics to publish to CloudWatch\. To select multiple metrics, press the **Ctrl** key while choosing\.

1. Choose **Apply**\.

Enabling CloudWatch custom metrics adds them to the list of metrics available on the [**Monitoring** page](environment-health-console.md)\.

## Configuring CloudWatch custom metrics using the EB CLI<a name="health-enhanced-cloudwatch-ebcli"></a>

You can use the EB CLI to configure custom metrics by saving your environment's configuration locally, adding an entry that defines the metrics to publish, and then uploading the configuration to Elastic Beanstalk\. You can apply the saved configuration to an environment during or after creation\.

**To configure CloudWatch custom metrics with the EB CLI and saved configurations**

1. Initialize your project folder with [eb init](eb-cli3-configuration.md)\.

1. Create an environment by running the [eb create](eb-cli3-getting-started.md) command\.

1. Save a configuration template locally by running the eb config save command\. The following example uses the `--cfg` option to specify the name of the configuration\.

   ```
   $ eb config save --cfg 01-base-state
   Configuration saved at: ~/project/.elasticbeanstalk/saved_configs/01-base-state.cfg.yml
   ```

1. Open the saved configuration file in a text editor\.

1. Under `OptionSettings` > `aws:elasticbeanstalk:healthreporting:system:`, add a `ConfigDocument` key to enable each of the CloudWatch metrics you want\. For example, the following `ConfigDocument` publishes `ApplicationRequests5xx` and `ApplicationRequests4xx` metrics at the environment level, and `ApplicationRequestsTotal` metrics at the instance level\.

   ```
   OptionSettings:
     ...
     aws:elasticbeanstalk:healthreporting:system:
       ConfigDocument:
         CloudWatchMetrics:
           Environment:
             ApplicationRequests5xx: 60
             ApplicationRequests4xx: 60
           Instance:
             ApplicationRequestsTotal: 60
         Version: 1
       SystemType: enhanced
   ...
   ```

   In the example, 60 indicates the number of seconds between measurements\. Currently, this is the only supported value\.
**Note**  
You can combine `CloudWatchMetrics` and `Rules` in the same `ConfigDocument` option setting\. `Rules` are described in [Configuring enhanced health rules for an environment](health-enhanced-rules.md)\.  
If you previously used `Rules` to configure enhanced health rules, then the configuration file that you retrieve using the eb config save command already has a `ConfigDocument` key with a `Rules` section\. *Do not delete it*—add a `CloudWatchMetrics` section into the same `ConfigDocument` option value\.

1. Save the configuration file and close the text editor\. In this example, the updated configuration file is saved with a name \(`02-cloudwatch-enabled.cfg.yml`\) that is different from the downloaded configuration file\. This creates a separate saved configuration when the file is uploaded\. You can use the same name as the downloaded file to overwrite the existing configuration without creating a new one\.

1. Use the eb config put command to upload the updated configuration file to Elastic Beanstalk\.

   ```
   $ eb config put 02-cloudwatch-enabled
   ```

   When using the eb config `get` and `put` commands with saved configurations, don't include the file extension\.

1. Apply the saved configuration to your running environment\.

   ```
   $ eb config --cfg 02-cloudwatch-enabled
   ```

   The `--cfg` option specifies a named configuration file that is applied to the environment\. You can save the configuration file locally or in Elastic Beanstalk\. If a configuration file with the specified name exists in both locations, the EB CLI uses the local file\.

## Providing custom metric config documents<a name="health-enhanced-cloudwatch-configdocument"></a>

The configuration \(config\) document for Amazon CloudWatch custom metrics is a JSON document that lists the metrics to publish at the environment and instance levels\. The following example shows a config document that enables all custom metrics available on Linux\.

```
{
  "CloudWatchMetrics": {
    "Environment": {
      "ApplicationLatencyP99.9": 60,
      "InstancesSevere": 60,
      "ApplicationLatencyP90": 60,
      "ApplicationLatencyP99": 60,
      "ApplicationLatencyP95": 60,
      "InstancesUnknown": 60,
      "ApplicationLatencyP85": 60,
      "InstancesInfo": 60,
      "ApplicationRequests2xx": 60,
      "InstancesDegraded": 60,
      "InstancesWarning": 60,
      "ApplicationLatencyP50": 60,
      "ApplicationRequestsTotal": 60,
      "InstancesNoData": 60,
      "InstancesPending": 60,
      "ApplicationLatencyP10": 60,
      "ApplicationRequests5xx": 60,
      "ApplicationLatencyP75": 60,
      "InstancesOk": 60,
      "ApplicationRequests3xx": 60,
      "ApplicationRequests4xx": 60
    },
    "Instance": {
      "ApplicationLatencyP99.9": 60,
      "ApplicationLatencyP90": 60,
      "ApplicationLatencyP99": 60,
      "ApplicationLatencyP95": 60,
      "ApplicationLatencyP85": 60,
      "CPUUser": 60,
      "ApplicationRequests2xx": 60,
      "CPUIdle": 60,
      "ApplicationLatencyP50": 60,
      "ApplicationRequestsTotal": 60,
      "RootFilesystemUtil": 60,
      "LoadAverage1min": 60,
      "CPUIrq": 60,
      "CPUNice": 60,
      "CPUIowait": 60,
      "ApplicationLatencyP10": 60,
      "LoadAverage5min": 60,
      "ApplicationRequests5xx": 60,
      "ApplicationLatencyP75": 60,
      "CPUSystem": 60,
      "ApplicationRequests3xx": 60,
      "ApplicationRequests4xx": 60,
      "InstanceHealth": 60,
      "CPUSoftirq": 60
    }
  },
  "Version": 1
}
```

For the AWS CLI, you pass the document as a value for the `Value` key in an option settings argument, which itself is a JSON object\. In this case, you must escape quotation marks in the embedded document\.

```
$ aws elasticbeanstalk validate-configuration-settings --application-name my-app --environment-name my-env --option-settings '[
    {
        "Namespace": "aws:elasticbeanstalk:healthreporting:system",
        "OptionName": "ConfigDocument",
        "Value": "{\"CloudWatchMetrics\": {\"Environment\": {\"ApplicationLatencyP99.9\": 60,\"InstancesSevere\": 60,\"ApplicationLatencyP90\": 60,\"ApplicationLatencyP99\": 60,\"ApplicationLatencyP95\": 60,\"InstancesUnknown\": 60,\"ApplicationLatencyP85\": 60,\"InstancesInfo\": 60,\"ApplicationRequests2xx\": 60,\"InstancesDegraded\": 60,\"InstancesWarning\": 60,\"ApplicationLatencyP50\": 60,\"ApplicationRequestsTotal\": 60,\"InstancesNoData\": 60,\"InstancesPending\": 60,\"ApplicationLatencyP10\": 60,\"ApplicationRequests5xx\": 60,\"ApplicationLatencyP75\": 60,\"InstancesOk\": 60,\"ApplicationRequests3xx\": 60,\"ApplicationRequests4xx\": 60},\"Instance\": {\"ApplicationLatencyP99.9\": 60,\"ApplicationLatencyP90\": 60,\"ApplicationLatencyP99\": 60,\"ApplicationLatencyP95\": 60,\"ApplicationLatencyP85\": 60,\"CPUUser\": 60,\"ApplicationRequests2xx\": 60,\"CPUIdle\": 60,\"ApplicationLatencyP50\": 60,\"ApplicationRequestsTotal\": 60,\"RootFilesystemUtil\": 60,\"LoadAverage1min\": 60,\"CPUIrq\": 60,\"CPUNice\": 60,\"CPUIowait\": 60,\"ApplicationLatencyP10\": 60,\"LoadAverage5min\": 60,\"ApplicationRequests5xx\": 60,\"ApplicationLatencyP75\": 60,\"CPUSystem\": 60,\"ApplicationRequests3xx\": 60,\"ApplicationRequests4xx\": 60,\"InstanceHealth\": 60,\"CPUSoftirq\": 60}},\"Version\": 1}"
    }
]'
```

For an `.ebextensions` configuration file in YAML, you can provide the JSON document as is\.

```
  option_settings:
    - namespace: aws:elasticbeanstalk:healthreporting:system
      option_name: ConfigDocument
      value: {
  "CloudWatchMetrics": {
    "Environment": {
      "ApplicationLatencyP99.9": 60,
      "InstancesSevere": 60,
      "ApplicationLatencyP90": 60,
      "ApplicationLatencyP99": 60,
      "ApplicationLatencyP95": 60,
      "InstancesUnknown": 60,
      "ApplicationLatencyP85": 60,
      "InstancesInfo": 60,
      "ApplicationRequests2xx": 60,
      "InstancesDegraded": 60,
      "InstancesWarning": 60,
      "ApplicationLatencyP50": 60,
      "ApplicationRequestsTotal": 60,
      "InstancesNoData": 60,
      "InstancesPending": 60,
      "ApplicationLatencyP10": 60,
      "ApplicationRequests5xx": 60,
      "ApplicationLatencyP75": 60,
      "InstancesOk": 60,
      "ApplicationRequests3xx": 60,
      "ApplicationRequests4xx": 60
    },
    "Instance": {
      "ApplicationLatencyP99.9": 60,
      "ApplicationLatencyP90": 60,
      "ApplicationLatencyP99": 60,
      "ApplicationLatencyP95": 60,
      "ApplicationLatencyP85": 60,
      "CPUUser": 60,
      "ApplicationRequests2xx": 60,
      "CPUIdle": 60,
      "ApplicationLatencyP50": 60,
      "ApplicationRequestsTotal": 60,
      "RootFilesystemUtil": 60,
      "LoadAverage1min": 60,
      "CPUIrq": 60,
      "CPUNice": 60,
      "CPUIowait": 60,
      "ApplicationLatencyP10": 60,
      "LoadAverage5min": 60,
      "ApplicationRequests5xx": 60,
      "ApplicationLatencyP75": 60,
      "CPUSystem": 60,
      "ApplicationRequests3xx": 60,
      "ApplicationRequests4xx": 60,
      "InstanceHealth": 60,
      "CPUSoftirq": 60
    }
  },
  "Version": 1
}
```