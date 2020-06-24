# Configuring AWS X\-Ray debugging<a name="environment-configuration-debugging"></a>

You can use the AWS Elastic Beanstalk console or a configuration file to run the AWS X\-Ray daemon on the instances in your environment\. X\-Ray is an AWS service that gathers data about the requests that your application serves, and uses it to construct a service map that you can use to identify issues with your application and opportunities for optimization\.

**Note**  
Some regions don't offer X\-Ray\. If you create an environment in one of these regions, you can't run the X\-Ray daemon on the instances in your environment\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

![\[The service map for a web API application that uses Amazon DynamoDB to store data\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/scorekeep-servicemap.png)

X\-Ray provides an SDK that you can use to instrument your application code, and a daemon application that relays debugging information from the SDK to the X\-Ray API\.

**Supported platforms**

You can use the X\-Ray SDK with the following Elastic Beanstalk platforms:
+ **Go** \- version 2\.9\.1 and later
+ **Java 8** \- version 2\.3\.0 and later
+ **Java 8 with Tomcat 8** \- version 2\.4\.0 and later
+ **Node\.js** \- version 3\.2\.0 and later
+ **Windows Server** \- all platform versions released on or after December 18th, 2016
+ **Python** \- version 2\.5\.0 and later

On supported platforms, you can use a configuration option to run the X\-Ray daemon on the instances in your environment\. You can enable the daemon in the [Elastic Beanstalk console](#environment-configuration-debugging-console) or by using a [configuration file](#environment-configuration-debugging-namespace)\.

To upload data to X\-Ray, the X\-Ray daemon requires IAM permissions in the **AWSXrayWriteOnlyAccess** managed policy\. These permissions are included in [the Elastic Beanstalk instance profile](concepts-roles-instance.md)\. If you don't use the default instance profile, see [Giving the Daemon Permission to Send Data to X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon.html#xray-daemon-permissions) in the *AWS X\-Ray Developer Guide*\.

Debugging with X\-Ray requires the use of the X\-Ray SDK\. See the [Getting Started with AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-gettingstarted.html) in the *AWS X\-Ray Developer Guide* for instructions and sample applications\.

If you use a platform version that doesn't include the daemon, you can still run it with a script in a configuration file\. For more information, see [ Downloading and Running the X\-Ray Daemon Manually \(Advanced\)](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon-beanstalk.html#xray-daemon-beanstalk-manual) in the *AWS X\-Ray Developer Guide*\.

**Topics**
+ [Configuring debugging](#environment-configuration-debugging-console)
+ [The aws:elasticbeanstalk:xray namespace](#environment-configuration-debugging-namespace)

## Configuring debugging<a name="environment-configuration-debugging-console"></a>

You can enable the X\-Ray daemon on a running environment in the Elastic Beanstalk console\.

**To enable debugging in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. In the **AWS X\-Ray** section, select **X\-Ray daemon**\.

1. Choose **Apply**\.

You can also enable this option during environment creation\. For more information, see [The create new environment wizard](environments-create-wizard.md)\.

## The aws:elasticbeanstalk:xray namespace<a name="environment-configuration-debugging-namespace"></a>

You can use the `XRayEnabled` option in the `aws:elasticbeanstalk:xray` namespace to enable debugging\.

To enable debugging automatically when you deploy your application, set the option in a [configuration file](ebextensions.md) in your source code, as follows\.

**Example \.ebextensions/debugging\.config**  

```
option_settings:
  aws:elasticbeanstalk:xray:
    XRayEnabled: true
```