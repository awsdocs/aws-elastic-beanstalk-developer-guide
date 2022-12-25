# Terminate an Elastic Beanstalk environment<a name="using-features.terminating"></a>

You can terminate a running AWS Elastic Beanstalk environment using the Elastic Beanstalk console\. By doing this, you avoid incurring charges for unused AWS resources\.   

**Note**  
You can always launch a new environment using the same version later\. If you have data from an environment that you want to preserve, set the database deletion policy to `Retain` before terminating the environment\. This keeps the database operational outside of Elastic Beanstalk\. After this, any Elastic Beanstalk environments must connect to it as an external database\. If you want to back up the data without keeping the database operational, set the deletion policy to take a snapshot of the database before terminating the environment\. For more information, see [Database lifecycle](using-features.managing.db.md#environments-cfg-rds-lifecycle) in the *Configuring environments* chapter of this guide\.

Elastic Beanstalk might fail to terminate your environment\. One common reason is that the security group of another environment has a dependency on the security group of the environment that you want to terminate\. For instructions on how to avoid this problem, see [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups) on the *EC2 Instances* page of this guide\.

## Elastic Beanstalk console<a name="using-features.terminating.CON"></a>

**To terminate an environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.
**Note**  
When you terminate your environment, the CNAME that's associated with the terminated environment is freed up to be used by anyone\.

   It takes a few minutes for Elastic Beanstalk to terminate the AWS resources that are running in the environment\. 

## AWS CLI<a name="using-features.terminating.CLI"></a>

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