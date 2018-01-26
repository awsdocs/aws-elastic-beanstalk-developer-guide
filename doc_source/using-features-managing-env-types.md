# Environment Types<a name="using-features-managing-env-types"></a>

In AWS Elastic Beanstalk, you can create a load\-balancing, autoscaling environment or a single\-instance environment\. The type of environment that you require depends on the application that you deploy\. For example, you can develop and test an application in a single\-instance environment to save costs and then upgrade that environment to a load\-balancing, autoscaling environment when the application is ready for production\.

**Note**  
A worker environment tier for a web application that processes background tasks doesn't include a load balancer\. However, a worker environment does effectively scale out by adding instances to the Auto Scaling group to process data from the Amazon SQS queue when the load necessitates it\.

## Load\-balancing, Autoscaling Environment<a name="autoscale-environ"></a>

A load\-balancing and autoscaling environment uses the Elastic Load Balancing and Amazon EC2 Auto Scaling services to provision the Amazon EC2 instances that are required for your deployed application\. Amazon EC2 Auto Scaling automatically starts additional instances to accommodate increasing load on your application\. If the load on your application decreases, Amazon EC2 Auto Scaling stops instances but always leaves your specified minimum number of instances running\. If your application requires scalability with the option of running in multiple Availability Zones, use a load\-balancing, autoscaling environment\. If you're not sure which environment type to select, you can pick one and, if required, switch the environment type later\.

## Single\-Instance Environment<a name="single-instance-environ"></a>

A single\-instance environment contains one Amazon EC2 instance with an Elastic IP address\. A single\-instance environment doesn't have a load balancer, which can help you reduce costs compared to a load\-balancing, autoscaling environment\. Although a single\-instance environment does use the Amazon EC2 Auto Scaling service, settings for the minimum number of instances, maximum number of instances, and desired capacity are all set to 1\. Consequently, new instances are not started to accommodate increasing load on your application\.

Use a single\-instance environment if you expect your production application to have low traffic or if you are doing remote development\. If you're not sure which environment type to select, you can pick one and, if required, you can switch the environment type later\. For more information, see [Changing Environment Type](#using-features.managing.changetype)\.

## Changing Environment Type<a name="using-features.managing.changetype"></a>

You can change your environment type to a single\-instance or load\-balancing, autoscaling environment by editing your environment's configuration\. In some cases, you might want to change your environment type from one type to another\. For example, let's say that you developed and tested an application in a single\-instance environment to save costs\. When your application is ready for production, you can change the environment type to a load\-balancing, autoscaling environment so that it can scale to meet the demands of your customers\.

**To change an environment's type**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Capacity** card, choose **Modify**\.  
![\[The capacity configuration card\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-scaling.png)

1. From the **Environment Type** list, select the type of environment that you want\.  
![\[The Auto Scaling group section of the modify capacity page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-capacity.png)

1. If your environment is in a VPC, select subnets to place Elastic Load Balancing and Amazon EC2 instances in\. Each Availability Zone that your application runs in must have both\. See  for details\.

1. Choose **Save**\.

   It can take several minutes for the environment to update while Elastic Beanstalk provisions AWS resources\.