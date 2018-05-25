# Using a Custom Amazon Machine Image \(AMI\)<a name="using-features.customenv"></a>

When you create an AWS Elastic Beanstalk environment, you can specify an Amazon Machine Image \(AMI\) to use instead of the standard Elastic Beanstalk AMI included in your platform configuration's solution stack\. A custom AMI can improve provisioning times when instances are launched in your environment if you need to install a lot of software that isn't included in the standard AMIs\.

Using [configuration files](ebextensions.md) is great for configuring and customizing your environment quickly and consistently\. Applying configurations, however, can start to take a long time during environment creation and updates\. If you do a lot of server configuration in configuration files, you can reduce this time by making a custom AMI that already has the software and configuration that you need\.

A custom AMI also allows you to make changes to low\-level components, such as the Linux kernel, that are difficult to implement or take a long time to apply in configuration files\. To create a custom AMI, launch an Elastic Beanstalk platform AMI in Amazon EC2, customize the software and configuration to your needs, and then stop the instance and save an AMI from it\.

## Creating a Custom AMI<a name="using-features.customenv.create"></a>

**To identify the base Elastic Beanstalk AMI**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Create an Elastic Beanstalk environment running your application\. For more information about how to launch an Elastic Beanstalk application, go to the [Getting Started Using Elastic Beanstalk](GettingStarted.md)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Instances** configuration card, note the value next to the **EC2 image ID** label\.  
![\[Instances configuration card with EC2 image ID highlighted\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-customami-imageid.png)

1. Terminate the environment\.

The value in the **Custom AMI ID** field is the stock Elastic Beanstalk AMI for the platform version, EC2 instance architecture, and AWS Region in which you created your environment\. If you need to create AMIs for multiple platforms, architectures or regions, repeat this process to identify the correct base AMI for each combination\.

**Note**  
Do not create an AMI from an instance that has been launched in an Elastic Beanstalk environment\. Elastic Beanstalk makes changes to instances during provisioning that can cause issues in the saved AMI\. Saving an image from an instance in an Elastic Beanstalk environment will also make the version of your application that was deployed to the instance a fixed part of the image\.

It is also possible to create a custom AMI from a community AMI that wasn't published by Elastic Beanstalk\. You can use the latest [Amazon Linux](https://aws.amazon.com//amazon-linux-ami/) AMI as a starting point\. When you launch an environment with a Linux AMI that isn't managed by Elastic Beanstalk, Elastic Beanstalk attempts to install platform software \(language, framework, proxy server, etc\.\) and additional components to support features such as [Enhanced Health Reporting](health-enhanced.md)\. 

**Note**  
AMIs that aren't managed by Elastic Beanstalk aren't supported for Windows Server\-based Elastic Beanstalk platforms\.

Although Elastic Beanstalk can use an AMI that isn't managed by Elastic Beanstalk, the increase in provisioning time that results from Elastic Beanstalk installing missing components can reduce or eliminate the benefits of creating a custom AMI in the first place\. Other Linux distributions might work with some troubleshooting but are not officially supported\. If your application requires a specific Linux distribution, one alternative is to create a Docker image and run it on the Elastic Beanstalk [single container Docker platform](docker-singlecontainer-deploy.md) or [multicontainer Docker platform](create_deploy_docker_ecs.md)\.

**To create a custom AMI**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Launch Instance**\.

1. Choose **Community AMIs**\.

1. If you identified a base Elastic Beanstalk or Amazon Linux AMI that you want to customize to create a custom AMI, enter its AMI ID in the search box, and then press **Enter**\.

   You can also search the list for another community AMI that suits your needs\.
**Note**  
We recommend that you choose an AMI that uses HVM virtualization\. These AMIs show **Virtualization type: hvm** in their description\.  

![\[AMI with HVM virtualization type listed on EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/using-features-customenv-hvm-ami.png)
For details about instance virtualization types, see [Linux AMI Virtualization Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Choose **Select** to select the AMI\.

1. Select an instance type, and then choose **Next: Configure Instance Details**\.

1. **\(Linux platforms\)** Expand the **Advanced Details** section and paste the following text in the **User Data** field\.

   ```
   #cloud-config
     repo_releasever: repository version number
     repo_upgrade: none
   ```

   The *repository version number* is the year and month version in the AMI name\. For example, AMIs based on the March 2015 release of Amazon Linux have a repository version number `2015.03`\. For an Elastic Beanstalk image, this matches the date shown in the solution stack name for your [platform configuration](concepts.platforms.md)\.
**Note**  
These settings configure the lock\-on\-launch feature\. This causes the AMI to use a fixed, specific repository version when it launches, and disables the automatic installation of security updates\. Both are required to use a custom AMI with Elastic Beanstalk\.

1. Proceed through the wizard to [launch the EC2 instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-an-instance.html)\. When prompted, select a key pair that you have access to so that you can connect to the instance for the next steps\.

1.  [Connect to the instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) with SSH or RDP\.

1. Perform any customizations you want\.

1. **\(Windows platforms\)** Run the EC2Config service Sysprep\. For information about EC2Config, see [Configuring a Windows Instance Using the EC2Config Service](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html)\. Ensure that Sysprep is configured to generate a random password that can be retrieved from the AWS Management Console\.

1. In the Amazon EC2 console, stop the EC2 instance\. Then on the **Instance Actions** menu, choose **Create Image \(EBS AMI\)**\.

1. To avoid incurring additional AWS charges, [terminate the EC2 instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)\.

**To use your custom AMI in an Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Instances** configuration card, choose **Modify**\.

1. For **Custom AMI ID**, type your custom AMI ID\.

1. Choose **Save**, and then choose **Apply**\.

When you create a new environment with the custom AMI, you should use the same platform configuration that you used as a base to create the AMI\. If you later apply a [platform update](using-features.platform.upgrade.md) to an environment using a custom AMI, Elastic Beanstalk attempts to apply the library and configuration updates during the bootstrapping process\.

## Cleaning Up a Custom AMI<a name="using-features.customenv.cleanup"></a>

When you are done with a custom AMI and don't need it to launch Elastic Beanstalk environments anymore, consider cleaning it up to minimize storage cost\. Cleaning up a custom AMI involves deregistering it from Amazon EC2 and deleting other associated resources\. For details, see [Deregistering Your Linux AMI](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/deregister-ami.html) or [Deregistering Your Windows AMI](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/deregister-ami.html)\.