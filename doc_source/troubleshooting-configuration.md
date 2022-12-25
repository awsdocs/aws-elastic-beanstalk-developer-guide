# Configuration<a name="troubleshooting-configuration"></a>

**Important**  
The *Let's Encrypt* cross\-signed DST Root CA X3 certificate *expired* on *September 30, 2021*\. Due to this, Beanstalk environments running on the Amazon Linux 2 and Amazon Linux AMI operating systems might not be able to connect to servers using *Let's Encrypt* certificates\.  
On October 3, 2021 Elastic Beanstalk released new platform versions for Amazon Linux AMI and Amazon Linux 2 with the updated CA certificates\. To receive these updates and address this issue turn on [Managed Updates](environment-platform-update-managed.md) or [update your platforms manually](using-features.platform.upgrade.md#using-features.platform.upgrade.config)\. For more information, see the [platform update release notes](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2021-10-03-linux.html) in the *AWS Elastic Beanstalk Release Notes*\.  
You can also apply the manual workarounds described in this [AWS Knowledge Center article](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-expired-certificate/)\. Since Elastic Beanstalk provides AMIs with locked GUIDs, we recommend that you use the sudo yum install command in the article\. Alternatively, you can also use the sudo sed command in the article if you prefer to manually modify the system in place\.

**Event:** *You cannot configure an Elastic Beanstalk environment with values for both the Elastic Load Balancing Target option and Application Healthcheck URL option*

The `Target` option in the `aws:elb:healthcheck` namespace is deprecated\. Remove the `Target` option namespace\) from your environment and try updating again\.

**Event:** *ELB cannot be attached to multiple subnets in the same AZ\.*

This message can be seen if you try to move a load balancer between subnets in the same Availability Zone\. Changing subnets on the load balancer requires moving it out of the original availability zone\(s\) and then back into the original with the desired subnets\. During the process, all of your instances will be migrated between AZs, causing significant downtime\. Instead, consider creating a new environment and [perform a CNAME swap](using-features.CNAMESwap.md)\.