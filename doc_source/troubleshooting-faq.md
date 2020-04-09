# FAQ<a name="troubleshooting-faq"></a>

**Question:** *How can I change my application URL from myapp\.us\-west\-2\.elasticbeanstalk\.com to www\.myapp\.com?*

In a DNS server, register a CNAME record such as **www\.mydomain\.com CNAME mydomain\.elasticbeanstalk\.com**\.

**Question:** *How do I specify a specific Availability Zone for my Elastic Beanstalk application?*

You can pick specific Availability Zones using the APIs, CLI, Eclipse plugin, or Visual Studio plugin\. For instructions about using the Elastic Beanstalk console to specify an Availability Zone, see [Auto Scaling group for your Elastic Beanstalk environment](using-features.managing.as.md)\.

**Question:** *How do I change my environment's instance type?*

On the environment configuration page, choose **Edit** in the **Instances** configuration category\. Select a new instance type and choose **Apply** to update your environment\. Elastic Beanstalk will terminate all running instances and replace them with new ones\.

**Question:** *Can I prevent Amazon EBS volumes from being deleted when instances are terminated?*

Instances in your environment use Amazon EBS for storage; however, the root volume is deleted when an instance is terminated by Auto Scaling\. It is not recommended to store state or other data on your instances\. If needed, you can prevent volumes from being deleted with the AWS CLI: `$ aws ec2 modify-instance-attribute -b '/dev/sdc=<vol-id>:false` as described in the [AWS CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-instance-attribute.html)\.

**Question:** *How do I delete personal information from my Elastic Beanstalk application?*

AWS resources that your Elastic Beanstalk application uses might store personal information\. When you terminate an environment, Elastic Beanstalk terminates the resources that it created\. Resources you added using [configuration files](ebextensions.md) are also terminated\. However, if you created AWS resources outside of your Elastic Beanstalk environment and associated them with your application, you might need to manually ensure that personal information that your application might have stored isn't unnecessarily retained\. Throughout this developer guide, wherever we discuss the creation of additional resources, we also mention when you should consider deleting them\.