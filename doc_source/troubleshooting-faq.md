# FAQ<a name="troubleshooting-faq"></a>

**Question:** *How can I change my application URL from myapp\.us\-west\-2\.elasticbeanstalk\.com to www\.myapp\.com?*

Register in a DNS server a CNAME record such as: www\.mydomain\.com CNAME mydomain\.elasticbeanstalk\.com\.

**Question:** *How do I specify a specific Availability Zone for my Elastic Beanstalk application\.*

You can pick specific AZs using the APIs, CLI, Eclipse plug\-in, or Visual Studio plug\-in\. For instructions about using the AWS Management Console to specify an Availability Zone, see [Your AWS Elastic Beanstalk Environment's Auto Scaling Group](using-features.managing.as.md)\.

**Question:** *How do I avoid getting charged for my applications?*

The default set of resources used by an Elastic Beanstalk do not incur charges in the Free Tier\. However, if you change the Amazon EC2 instance type, add EC2 instances, or run resources outside of your Elastic Beanstalk environment, charges may be accrued\. For information about the free tier, see [http://aws\.amazon\.com/free](http://aws.amazon.com/free)\. If you have questions about your account, contact our [ customer service team](https://aws-portal.amazon.com/gp/aws/html-forms-controller/contactus/aws-account-and-billing) directly\.

**Question:** *Can I receive notifications by SMS?*

If you specify an SMS email address, such as one constructed on [http://www\.makeuseof\.com/tag/email\-to\-sms](http://www.makeuseof.com/tag/email-to-sms/), you will receive the notifications by SMS\. To subscribe to more than one email address, you can use the Elastic Beanstalk command line to register an SNS topic with an environment\.

**Question:** *How do I change my environment's instance type?*

In the **Web Tier** section of the environment configuration screen, choose the gear icon on the **Instances** card\. Select a new instance type and click **Apply** to update your environment\. Elastic Beanstalk will terminate all running instances and replace them with new ones\.

**Question:** *Can I prevent EBS volumes from being deleted when instances are terminated?*

Instances in your environment use EBS for storage; however, the root volume is deleted when an instance is terminated by Auto Scaling\. It is not recommended to store state or other data on your instances\. If needed, you can prevent volumes from being deleted with the AWS CLI: `$ aws ec2 modify-instance-attribute -b '/dev/sdc=<vol-id>:false` as described in the [AWS CLI Reference](http://docs.aws.amazon.com/cli/latest/reference/ec2/modify-instance-attribute.html)\.