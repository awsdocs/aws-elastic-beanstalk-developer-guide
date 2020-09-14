# Using Elastic Beanstalk with Amazon Elastic File System<a name="services-efs"></a>

With Amazon Elastic File System \(Amazon EFS\), you can create network file systems that can be mounted by instances across multiple Availability Zones\. An Amazon EFS file system is an AWS resource that uses security groups to control access over the network in your default or custom VPC\.

In an Elastic Beanstalk environment, you can use Amazon EFS to create a shared directory that stores files uploaded or modified by users of your application\. Your application can treat a mounted Amazon EFS volume like local storage, so you don't have to change your application code to scale up to multiple instances\.

For more information about Amazon EFS, see the [Amazon Elastic File System User Guide](https://docs.aws.amazon.com/efs/latest/ug/)\.

**Topics**
+ [Configuration files](#services-efs-configs)
+ [Encrypted file systems](#services-efs-encrypted)
+ [Sample applications](#services-efs-samples)
+ [Cleaning up file systems](#services-efs-cleanup)

## Configuration files<a name="services-efs-configs"></a>

Elastic Beanstalk provides [configuration files](ebextensions.md) that you can use to create and mount Amazon EFS file systems\. You can create an Amazon EFS volume as part of your environment, or mount an Amazon EFS volume that you created independently of Elastic Beanstalk\.
+ **[storage\-efs\-createfilesystem\.config](https://github.com/awslabs/elastic-beanstalk-samples/blob/master/configuration-files/aws-provided/instance-configuration/storage-efs-createfilesystem.config)** – Uses the `Resources` key to create a new file system and mount points in Amazon EFS\. All instances in your environment can connect to the same file system for shared, scalable storage\. Use `storage-efs-mountfilesystem.config` to mount the file system on each instance\.
**Internal resources**  
Any resources that you create with configuration files are tied to the lifecycle of your environment and will be lost if you terminate your environment or remove the configuration file\.
+ **[storage\-efs\-mountfilesystem\.config](https://github.com/awslabs/elastic-beanstalk-samples/blob/master/configuration-files/aws-provided/instance-configuration/storage-efs-mountfilesystem.config)** – Mount an Amazon EFS file system to a local path on the instances in your environment\. You can create the volume as part of the environment with `storage-efs-createfilesystem.config`, or external to your environment by using the Amazon EFS console, AWS CLI, or AWS SDK\.

To use the configuration files, start by creating your Amazon EFS file system with `storage-efs-createfilesystem.config`\. Follow the instructions in the configuration file and add it to the [\.ebextensions](ebextensions.md) directory in your source code to create the file system in your VPC\.

Deploy your updated source code to your Elastic Beanstalk environment to confirm that the file system is created successfully\. Then, add the `storage-efs-mountfilesystem.config` to mount the file system to the instances in your environment\. Doing this in two separate deployments ensures that if the mount operation fails, the file system is left intact\. If you do both in the same deployment, an issue with either step will cause the file system to terminate when the deployment fails\.

## Encrypted file systems<a name="services-efs-encrypted"></a>

Amazon EFS supports encrypted file systems\. The [https://github.com/awslabs/elastic-beanstalk-samples/blob/master/configuration-files/aws-provided/instance-configuration/storage-efs-createfilesystem.config](https://github.com/awslabs/elastic-beanstalk-samples/blob/master/configuration-files/aws-provided/instance-configuration/storage-efs-createfilesystem.config) configuration file discussed in this topic defines two custom options that you can use to create an Amazon EFS encrypted file system\. For details, follow the instructions in the configuration file\.

## Sample applications<a name="services-efs-samples"></a>

Elastic Beanstalk also provides sample applications that use Amazon EFS for shared storage\. The two projects are configuration files that you can use with a standard WordPress or Drupal installer to run a blog or other content management system in a load\-balanced environment\. When a user uploads a photo or other media, it is stored on an Amazon EFS file system, avoiding the need to use a plugin to store uploaded files in Amazon S3\.
+ **[Load\-balanced WordPress](https://github.com/awslabs/eb-php-wordpress)** – Configuration files for installing WordPress securely and running it in a load\-balanced Elastic Beanstalk environment\.
+ **[Load\-balanced Drupal](https://github.com/awslabs/eb-php-drupal)** – Configuration files and instructions for installing Drupal securely and running it in a load\-balanced Elastic Beanstalk environment\. 

## Cleaning up file systems<a name="services-efs-cleanup"></a>

If you created an Amazon EFS file system using a configuration file as part of your Elastic Beanstalk environment, Elastic Beanstalk removes the file system when you terminate the environment\. To minimize storage costs of a running application, routinely delete files that your application doesn't need, or ensure that the application code maintains file lifecycle correctly\.

In addition, if you created an Amazon EFS file system outside of an Elastic Beanstalk environment and mounted it to the environment's instances, be aware that Elastic Beanstalk doesn't remove the file system when you terminate the environment\. To ensure that personal information isn't unnecessarily retained, delete files that your application stored if you don't need them anymore, or remove the file system\.