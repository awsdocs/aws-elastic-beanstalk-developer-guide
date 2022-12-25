# FAQ<a name="troubleshooting-faq"></a>

**Question:** *How can I change my application URL from myapp\.us\-west\-2\.elasticbeanstalk\.com to www\.myapp\.com?*

In a DNS server, register a CNAME record such as **www\.mydomain\.com CNAME mydomain\.elasticbeanstalk\.com**\.

**Question:** *How do I specify a specific Availability Zone for my Elastic Beanstalk application?*

You can pick a specific Availability Zone by using the APIs, CLI, Eclipse plugin, or Visual Studio plugin\. For instructions about using the Elastic Beanstalk console to specify an Availability Zone, see [Auto Scaling group for your Elastic Beanstalk environment](using-features.managing.as.md)\.

**Question:** *How do I change my environment's instance type?*

To change your environment's instance type go to the environment configuration page and choose **Edit** in the **Instances** configuration category\. Then, select a new instance type and choose **Apply** to update your environment\. After this, Elastic Beanstalk terminates all running instances and replaces them with new ones\.

**Question:** *How do I determine if anyone made configuration changes to an environment?*

To see this information, in the navigation pane of the Elastic Beanstalk console choose **Change history** to display a list of configuration changes for all environments\. This list includes the date and time of the change, the configuration parameter and value it was changed to, and the IAM user that made the change\. For more information, see [Change history](using-features.changehistory.md)\. 

**Question:** *Can I prevent Amazon EBS volumes from being deleted when instances are terminated?*

Instances in your environment use Amazon EBS for storage; however, the root volume is deleted when an instance is terminated by Auto Scaling\. We don'trecommend that you store state or other data on your instances\. If needed, you can prevent volumes from being deleted with the AWS CLI: `$ aws ec2 modify-instance-attribute -b '/dev/sdc=<vol-id>:false` as described in the [AWS CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-instance-attribute.html)\.

**Question:** *How do I delete personal information from my Elastic Beanstalk application?*

AWS resources that your Elastic Beanstalk application uses might store personal information\. When you terminate an environment, Elastic Beanstalk terminates the resources that it created\. Resources you added using [configuration files](ebextensions.md) are also terminated\. However, if you created AWS resources outside of your Elastic Beanstalk environment and associated them with your application, you might need to manually check that personal information that your application might have stored isn't retained\. Throughout this developer guide, whenever we discuss creating additional resources, we also mention when you should consider deleting them\.