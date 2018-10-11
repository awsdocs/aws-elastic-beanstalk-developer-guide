# Configuring HTTP to HTTPS Redirection<a name="configuring-https-httpredirect"></a>

In [Configuring HTTPS for Your Elastic Beanstalk Environment](configuring-https.md) and its subtopics, we discuss configuring your Elastic Beanstalk environment to use HTTPS to ensure traffic encryption into your application\. This topic describes how to elegantly handle HTTP traffic to your application if end users still initiate it\. You do this by configuring *HTTP to HTTPS redirection*, sometimes referred to as *forcing HTTPS*\.

To configure redirection, you first configure your environment to use HTTPS traffic\. Then you configure the web servers on the environment's instances to redirect HTTP traffic to HTTPS by responding with an HTTP redirection response status\.

**To configure HTTP to HTTPS redirection**

1. Configure your environment to [terminate HTTPS connections at the instance](https-singleinstance.md)\.

1. If your environment is load balanced, configure the load balancer for [end\-to\-end encryption](configuring-https-endtoend.md) or [TCP passthrough](https-tcp-passthrough.md)\.

1. Configure your Amazon Elastic Compute Cloud \(Amazon EC2\) instances to redirect HTTP traffic to HTTPS\. This configuration depends on your environment's platform\. Find the folder for your platform in the [https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/https-redirect](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/https-redirect) collection on GitHub, and use the example configuration file in that folder\.

**Note**  
If your environment uses [Elastic Load Balancing health checks](using-features.healthstatus.md#using-features.healthstatus.understanding), the load balancer expects a healthy instance to respond to the HTTP health check messages with HTTP 200 \(OK\) responses\. Therefore, your web server shouldn't redirect these messages to HTTPS\. The example configuration files in [https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/https-redirect](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/https-redirect) handle this requirement correctly\.