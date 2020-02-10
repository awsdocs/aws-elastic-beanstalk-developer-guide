# Managing multiple AWS accounts<a name="create_deploy_Java.accounts"></a>

You might want to set up different AWS accounts to perform different tasks, such as testing, staging, and production\. You can use the AWS Toolkit for Eclipse to add, edit, and delete accounts easily\.

**To add an AWS account with the AWS Toolkit for Eclipse**

1. In Eclipse, make sure the toolbar is visible\. On the toolbar, click the arrow next to the AWS icon and select **Preferences**\.

1. Click **Add account**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-add-account.png)

1. In the **Account Name** text box, type the display name for the account\. 

1. In the **Access Key ID** text box, type your AWS access key ID\. 

1. In the **Secret Access Key** text box, type your AWS secret key\. 

   For API access, you need an access key ID and secret access key\. Use IAM user access keys instead of AWS account root user access keys\. For more information about creating access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\. 

1. Click **OK**\. 

**To use a different account to deploy an application to Elastic Beanstalk**

1. In the Eclipse toolbar, click the arrow next to the AWS icon and select **Preferences**\. 

1. For **Default Account**, select the account you want to use to deploy applications to Elastic Beanstalk\. 

1. Click **OK**\. 

1. In the **Project Explorer** pane, right\-click the application you want to deploy, and then select **Amazon Web Services** > **Deploy to Elastic Beanstalk**\. 