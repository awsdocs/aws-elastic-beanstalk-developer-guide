# Terminate an Elastic Beanstalk Environment<a name="using-features.terminating"></a>

You can terminate a running AWS Elastic Beanstalk environment using the Elastic Beanstalk console to avoid incurring charges for unused AWS resources\. For more information about terminating an environment using the AWS Toolkit for Eclipse, see [Terminating an Environment](create_deploy_Java.terminating.md)\.

**Note**  
You can always launch a new environment using the same version later\. If you have data from an environment that you want to preserve, create a snapshot of your current database instance before you terminate the environment\. You can use it later as the basis for new DB instance when you create a new environment\. For more information, see [Creating a DB Snapshot](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html) in the [Amazon Relational Database Service User Guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)\.

Elastic Beanstalk might fail to terminate your environment\. One common reason for this failure is that another environment's security group has a dependency on the security group of the environment you're trying to terminate\. A way to avoid this condition is described in [Security Groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups) on the *EC2 Instances* page of this guide\.

## Elastic Beanstalk console<a name="using-features.terminating.CON"></a>

**To terminate an environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. From the region list, select the region that includes the environment that you want to terminate\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then select **Terminate Environment**\.  
![\[Actions menu on the Dashboard page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-dashboard-action.png)

1. Use the on\-screen dialog box to confirm environment termination\.
**Note**  
When you terminate your environment, the CNAME associated with the terminated environment becomes available for anyone to use\. 

   It takes a few minutes for Elastic Beanstalk to terminate the AWS resources running in the environment\. 

## CLI<a name="using-features.terminating.CLI"></a>

**To terminate an environment**
+ Run the following command\.

  ```
  $ aws elasticbeanstalk terminate-environment --environment-name my-env
  ```

## API<a name="using-features.terminating.API"></a>

**To terminate an environment**
+ Call `TerminateEnvironment` with the following parameter:

  `EnvironmentName` = `SampleAppEnv`

  ```
  1. https://elasticbeanstalk.us-west-2.amazon.com/?EnvironmentName=SampleAppEnv
  2. &Operation=TerminateEnvironment
  3. &AuthParams
  ```