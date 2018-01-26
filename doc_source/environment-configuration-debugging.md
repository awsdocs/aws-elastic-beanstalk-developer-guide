# Configuring AWS X\-Ray Debugging<a name="environment-configuration-debugging"></a>

You can use the AWS Elastic Beanstalk console or a configuration file to run the AWS X\-Ray daemon on the instances in your environment\. AWS X\-Ray is an AWS service that gathers data about the requests that your application serves, and uses it to construct a service map that you can use to identify issues with your application and opportunities for optimization\.

**Note**  
Some regions don't offer AWS X\-Ray\. If you create an environment in one of these regions, you can't run the AWS X\-Ray daemon on the instances in your environment\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

![\[The service map for a web API application that uses Amazon DynamoDB to store data\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/scorekeep-servicemap.png)

AWS X\-Ray provides an SDK that you can use to instrument your application code, and a daemon application that relays debugging information from the SDK to the X\-Ray API\.

**Supported platforms**

You can use the AWS X\-Ray SDK with the following Elastic Beanstalk platform configurations:

+ **Java 8** \- version 2\.3\.0 and later

+ **Java 8 with Tomcat 8** \- version 2\.4\.0 and later

+ **Node\.js** \- version 3\.2\.0 and later

+ **Python** \- version 2\.5\.0 and later

+ **Windows Server** \- all configurations, starting December 9th, 2016

On supported platforms, you can use a configuration option to run the X\-Ray daemon on the instances in your environment\. You can enable the daemon in the Elastic Beanstalk console or by using a configuration file\.

**Note**  
To upload data to X\-Ray, the X\-Ray daemon requires IAM permissions in the **AWSXrayWriteOnlyAccess** managed policy\. These permissions are included in the Elastic Beanstalk instance profile\. If you don't use the default instance profile, see [Giving the Daemon Permission to Send Data to X\-Ray](http://docs.aws.amazon.com/xray/latest/devguide/xray-daemon.html#xray-daemon-permissions)\.

Debugging with AWS X\-Ray requires the use of the AWS X\-Ray SDK\. See the [AWS X\-Ray Developer Guide](http://docs.aws.amazon.com//xray/latest/devguide/xray-gettingstarted.html) for instructions and sample applications\.

If you use a platform configuration that doesn't include the daemon, you can still run it with a script in a configuration file\. For more information, see [ Downloading and Running the X\-Ray Daemon Manually \(Advanced\)](http://docs.aws.amazon.com//xray/latest/devguide/xray-daemon-beanstalk.html#xray-daemon-beanstalk-manual) in the *AWS X\-Ray Developer Guide*\.


+ [Configuring Debugging](#environment-configuration-debugging-console)
+ [The aws:elasticbeanstalk:xray Namespace](#environment-configuration-debugging-namespace)

## Configuring Debugging<a name="environment-configuration-debugging-console"></a>

You can enable the AWS X\-Ray daemon on a running environment in the Elastic Beanstalk console\.

**To enable debugging in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. For **X\-Ray Daemon**, choose **Enabled**\.

1. Choose **Save**, and then choose **Apply**\.

You can also enable this option during environment creation\. For more information, see \.

## The aws:elasticbeanstalk:xray Namespace<a name="environment-configuration-debugging-namespace"></a>

You can use the `XRayEnabled` option in the `aws:elasticbeanstalk:xray` namespace to enable debugging\.

To enable debugging automatically when you deploy your application, set the option in a configuration file in your source code, as follows\.

**Example \.ebextensions/debugging\.config**  

```
option_settings:
  aws:elasticbeanstalk:xray:
    XRayEnabled: true
```