# Packer Instance Cleanup<a name="custom-platforms-packercleanup"></a>

In certain circumstances, such as killing the Packer builder process before it has finished, instances launched by Packer are not cleaned up\. These instances are not part of the Elastic Beanstalk environment and can only be viewed and terminated using the Amazon EC2 service\.

**To manually clean up these instances**

1. Open the [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)

1. Make sure you are in the same region in which you created the instance with Packer\.

1. Under **Resources** select *N ***Running Instances**, where *N* indicates the number of running instances\.

1. Click in the query text box\.

1. Select the **Name** tag\.

1. Type **packer**\.

   The query should look like: **tag:Name: packer**

1. Select any instances that match the query\.

1. If the **Instance State** is **running**, select **Actions**, **Instance State**, **Stop**, then **Actions**, **Instance State**, **Terminate**\.