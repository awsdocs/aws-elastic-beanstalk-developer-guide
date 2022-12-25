# Example: Using custom Amazon CloudWatch metrics<a name="customize-containers-cw"></a>

Amazon CloudWatch is a web service that enables you to monitor, manage, and publish various metrics, as well as configure alarm actions based on data from metrics\. You can define custom metrics for your own use, and Elastic Beanstalk will push those metrics to Amazon CloudWatch\. Once Amazon CloudWatch contains your custom metrics, you can view those in the Amazon CloudWatch console\.

The Amazon CloudWatch agent is an open-source project under the MIT license that enables CloudWatch metric and log collection from Amazon EC2 instances and on-premises servers across operating systems. The agent supports metrics collected at the system level as well as custom log and metric collection from your applications or services. This agent replaces the [Amazon CloudWatch Monitoring Scripts](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/mon-scripts.html), which are now deprecated. For more information about the Amazon CloudWatch agent, go to [Collecting metrics and logs from Amazon EC2 instances and on-premises servers with the CloudWatch agent ](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *Amazon CloudWatch User Guide*\.

**Note**  
Elastic Beanstalk [Enhanced Health Reporting](health-enhanced.md) has native support for publishing a wide range of instance and environment metrics to CloudWatch\. See [Publishing Amazon CloudWatch custom metrics for an environment](health-enhanced-cloudwatch.md) for details\.

**Topics**
- [Example: Using custom Amazon CloudWatch metrics<a name="customize-containers-cw"></a>](#example-using-custom-amazon-cloudwatch-metrics)
  - [\.Ebextensions configuration file<a name="customize-containers-cw-update-roles"></a>](#ebextensions-configuration-file)
  - [Permissions<a name="customize-containers-cw-policy"></a>](#permissions)
  - [Viewing metrics in the CloudWatch console<a name="customize-containers-cw-console"></a>](#viewing-metrics-in-the-cloudwatch-console)

## \.Ebextensions configuration file<a name="customize-containers-cw-update-roles"></a>

This example uses files and commands in an \.ebextensions configuration file to configure and run the Amazon CloudWatch agent on the Amazon Linux 2 platform\. The agent is prepackaged with Amazon Linux 2--if you are using a different operating system, additional steps for installing the agent may be necessary. See [Installing the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html) for details.

To use this sample, save it to a file named `cloudwatch.config` in a directory named `.ebextensions` at the top level of your project directory, then deploy your application using the Elastic Beanstalk console \(include the \.ebextensions directory in your [source bundle](applications-sourcebundle.md)\) or the [EB CLI](eb-cli3.md)\.

For more information about configuration files, see [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)\.

**\.ebextensions/cloudwatch\.config**

```
files:  
  "/opt/aws/amazon-cloudwatch-agent/bin/config.json": 
    mode: "000600"
    owner: root
    group: root
    content: |
      {
        "agent": {
          "metrics_collection_interval": 60,
          "run_as_user": "root"
        },
        "metrics": {
          "append_dimensions": {
            "AutoScalingGroupName": "${aws:AutoScalingGroupName}"
          },
          "metrics_collected": {
            "mem": {
              "measurement": [
                "used_percent"
              ]
            }
          }
        }
      }

container_commands:
  start_cloudwatch_agent: 
    command: /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

This file has two sections:
- `files`: This section adds the agent configuration file, which tells the agent what metrics and/or logs to send to Amazon CloudWatch. In this example, we are only sending the `mem_used_percent` metric. See [Metrics collected by the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/metrics-collected-by-CloudWatch-agent.html) for a complete listing of system-level metrics supported by the Amazon CloudWatch agent.
- `container_commands`: This section contains a command that starts the agent using the configuration file. See [Customizing software on Linux servers](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/customize-containers-ec2.html#linux-container-commands) from the AWS Elastic Beanstalk Developer Guide for more details on `container_commands`.

## Permissions<a name="customize-containers-cw-policy"></a>

The instances in your environment need the proper IAM permissions in order to publish custom Amazon CloudWatch metrics using the Amazon CloudWatch agent\. You can grant permissions to your environment's instances by adding them to the environment's [instance profile](concepts-roles-instance.md)\. You can add permissions to the instance profile before or after deploying your application\.

**To grant permissions to publish CloudWatch metrics**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

2. In the navigation pane, choose **Roles**\.

3. Choose your environment's instance profile role\. By default, when you create an environment with the Elastic Beanstalk console or [EB CLI](eb-cli3.md), this is `aws-elasticbeanstalk-ec2-role`\.

4. Choose the **Permissions** tab\.

5. Under **Permissions Policies**, in the **Permissions** section, choose **Attach policies**\.

6. Under **Attach Permissions**, choose the AWS managed policy **CloudWatchAgentServerPolicy**, and click **Attach policy**.

   For more information about managing policies, see [Working with Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) in the *IAM User Guide*\.

## Viewing metrics in the CloudWatch console<a name="customize-containers-cw-console"></a>

After deploying the CloudWatch configuration file to your environment, check the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/home) to view your metrics\. Custom metrics will be located in the **CWAgent** namespace\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-container-cw.png)