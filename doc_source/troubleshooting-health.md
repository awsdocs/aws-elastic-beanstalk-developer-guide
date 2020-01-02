# Health<a name="troubleshooting-health"></a>

**Event:** *CPU Utilization Exceeds 95\.00%*

Try [running more instances](using-features.managing.as.md), or [choose a different instance type](using-features.managing.ec2.md)\.

**Event:** *Elastic Load Balancer awseb\-*myapp* Has Zero Healthy Instances*

If your application appears to be working, make sure that your application’s health check URL is configured correctly\. If not, check the Health screen and environment logs for more information\.

**Event:** *Elastic Load Balancer awseb\-*myapp* Cannot Be Found*

Your environment's load balancer may have been removed out\-of\-band\. Only make changes to your environment's resources with the configuration options and [extensibility](ebextensions.md) provided by Elastic Beanstalk\. Rebuild your environment or launch a new one\.

**Event:** *EC2 Instance Launch Failure\. Waiting for a New EC2 Instance to Launch\.\.\.*

Availability for your environment's instance type may be low, or you may have reached the instance quota for your account\. Check the [service health dashboard](http://status.aws.amazon.com/) to ensure that the Elastic Compute Cloud \(Amazon EC2\) service is green, or [request a quota increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-ec2-instances)\.