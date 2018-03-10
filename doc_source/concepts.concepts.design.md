# Design Considerations<a name="concepts.concepts.design"></a>

 Because applications deployed using Elastic Beanstalk run on Amazon cloud resources, you should keep several things in mind when designing your application: *scalability*, *security*, *persistent storage*, *fault tolerance*, *content delivery*, *software updates and patching*, and *connectivity*\. For a comprehensive list of technical AWS whitepapers, covering topics such as architecture, security and economics, go to [AWS Cloud Computing Whitepapers](https://aws.amazon.com/whitepapers/)\. 

## Scalability<a name="concepts.concepts.design.scalability"></a>

When you're operating in a physical hardware environment, as opposed to a cloud environment, you can approach scalability two ways—you can scale up \(vertical scaling\) or scale out \(horizontal scaling\)\. The scale\-up approach requires an investment in powerful hardware as the demands on the business increase, whereas the scale\-out approach requires following a distributed model of investment, so hardware and application acquisitions are more targeted, data sets are federated, and design is service\-oriented\. The scale\-up approach could become very expensive, and there's still the risk that demand could outgrow capacity\. Although the scale\-out approach is usually more effective, it requires predicting the demand at regular intervals and deploying infrastructure in chunks to meet demand\. This approach often leads to unused capacity and requires careful monitoring\.

By moving to the cloud you can bring the use of your infrastructure into close alignment with demand by leveraging the elasticity of the cloud\. Elasticity is the streamlining of resource acquisition and release, so that your infrastructure can rapidly scale in and scale out as demand fluctuates\. To implement elasticity, configure your Auto Scaling settings to scale up or down based on metrics from the resources in your environment \(utilization of the servers or network I/O, for instance\)\. You can use Auto Scaling to automatically add compute capacity when usage rises and remove it when usage drops\. Publish system metrics \(CPU, memory, disk I/O, network I/O\) to Amazon CloudWatch and configure alarms to trigger Auto Scaling actions or send notifications\. For more instructions on configuring Auto Scaling, see [Auto Scaling Group for Your AWS Elastic Beanstalk Environment](using-features.managing.as.md)\.

Elastic Beanstalk applications should also be as *stateless* as possible, using loosely coupled, fault\-tolerant components that can be scaled out as needed\. For more information about designing scalable application architectures for AWS, read the [Architecting for the Cloud: Best Practices](http://media.amazonwebservices.com/AWS_Cloud_Best_Practices.pdf) whitepaper\.

## Security<a name="concepts.concepts.design.security"></a>

Security on AWS is a [shared responsibility](https://aws.amazon.com/compliance/shared-responsibility-model/)\. AWS protects the physical resources in your environment and ensure that the cloud is a safe place for you to run applications\. You are responsible for the security of data coming in and out of your Elastic Beanstalk environment and the security of your application\.

Configure SSL to protect information from your clients\. You will need a certificate from an external certification authority such as VeriSign or Entrust\. The public key included in the certificate authenticates your server to the browser and serves as the basis for creating the shared session key used to encrypt the data in both directions\. For instructions on creating, uploading, and assigning an SSL certificate to your environment, see [Configuring HTTPS for your Elastic Beanstalk Environment](configuring-https.md)\.

When you configure an SSL certificate for your environment, data is encrypted between the client and your environment's Elastic Load Balancing load balancer\. By default, encryption is terminated at the load balancer and traffic between the load balancer and Amazon EC2 instances is unencrypted\.

## Persistent Storage<a name="concepts.concepts.design.storage"></a>

 Elastic Beanstalk applications run on Amazon EC2 instances that have no persistent local storage\. When the Amazon EC2 instances terminate, the local file system is not saved, and new Amazon EC2 instances start with a default file system\. You should design your application to store data in a persistent data source\. Amazon Web Services offers a number of persistent storage services that you can leverage for your application\. The following table lists them\.


| Storage service | Service documentation | Elastic Beanstalk integration | 
| --- | --- | --- | 
| [Amazon S3](https://aws.amazon.com/s3/) | [Amazon Simple Storage Service Documentation](https://aws.amazon.com/documentation/s3/) | [Using Elastic Beanstalk with Amazon Simple Storage Service](AWSHowTo.S3.md) | 
| [Amazon Elastic File System](https://aws.amazon.com/efs/) | [Amazon Elastic File System Documentation](https://aws.amazon.com/documentation/efs/) | [Using Elastic Beanstalk with Amazon Elastic File System](services-efs.md) | 
| [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) |  [Amazon Elastic Block Store](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) [Feature Guide: Elastic Block Store](https://aws.amazon.com/articles/1667)  |  | 
| [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) | [Amazon DynamoDB Documentation](https://aws.amazon.com/documentation/dynamodb/) | [Using Elastic Beanstalk with DynamoDB](AWSHowTo.dynamoDB.md) | 
| [Amazon Relational Database Service \(RDS\)](https://aws.amazon.com/rds/) | [Amazon Relational Database Service Documentation](https://aws.amazon.com/documentation/rds/) | [Using Elastic Beanstalk with Amazon Relational Database Service](AWSHowTo.RDS.md) | 

## Fault Tolerance<a name="concepts.concepts.design.faulttolerance"></a>

As a rule of thumb, you should be a pessimist when designing architecture for the cloud\. Always design, implement, and deploy for automated recovery from failure\. Use multiple Availability Zones for your Amazon EC2 instances and for Amazon RDS\. Availability Zones are conceptually like logical data centers\. Use Amazon CloudWatch to get more visibility into the health of your Elastic Beanstalk application and take appropriate actions in case of hardware failure or performance degradation\. Configure your Auto Scaling settings to maintain your fleet of Amazon EC2 instances at a fixed size so that unhealthy Amazon EC2 instances are replaced by new ones\. If you are using Amazon RDS, then set the retention period for backups, so that Amazon RDS can perform automated backups\.

## Content Delivery<a name="concepts.concepts.design.cloudfront"></a>

When users connect to your website, their requests may be routed through a number of individual networks\. As a result users may experience poor performance due to high latency\. Amazon CloudFront can help ameliorate latency issues by distributing your web content \(such as images, video, and so on\) across a network of edge locations around the world\. End users are routed to the nearest edge location, so content is delivered with the best possible performance\. CloudFront works seamlessly with Amazon S3, which durably stores the original, definitive versions of your files\. For more information about Amazon CloudFront, see [https://aws\.amazon\.com/cloudfront](https://aws.amazon.com/cloudfront/)\. 

## Software Updates and Patching<a name="concepts.concepts.design.updates"></a>

Elastic Beanstalk periodically updates its platform configurations with new software and patches\. Elastic Beanstalk does not upgrade running environments to new configuration versions automatically, but you can initiate a [platform upgrade](using-features.platform.upgrade.md) to update your running environment in place\. Platform upgrades use [rolling updates](using-features.rollingupdates.md) to keep your application available by applying changes in batches\.

## Connectivity<a name="concepts.concepts.design.connectivity"></a>

Elastic Beanstalk needs to be able to connect to the instances in your environment to complete deployments\. When you deploy an Elastic Beanstalk application inside an Amazon VPC, the configuration required to enable connectivity depends on the type of Amazon VPC environment you create:

+ For single\-instance environments, no additional configuration is required because Elastic Beanstalk assigns each Amazon EC2 instance a public Elastic IP address that enables the instance to communicate directly with the Internet\.

+ For load\-balancing, autoscaling environments in an Amazon VPC with both public and private subnets, you must do the following: 

  + Create a load balancer in the public subnet to route inbound traffic from the Internet to the Amazon EC2 instances\.

  + Create a network address translation \(NAT\) device to route outbound traffic from the Amazon EC2 instances to the Internet\.

  + Create inbound and outbound routing rules for the Amazon EC2 instances inside the private subnet\.

  + If using a NAT instance, configure the security groups for the NAT instance and Amazon EC2 instances to enable Internet communication\.

+ For a load\-balancing, autoscaling environment in an Amazon VPC that has one public subnet, no additional configuration is required because the Amazon EC2 instances are configured with a public IP address that enables the instances to communicate with the Internet\.

For more information about using Elastic Beanstalk with Amazon VPC, see [Using Elastic Beanstalk with Amazon Virtual Private Cloud](vpc.md)\.