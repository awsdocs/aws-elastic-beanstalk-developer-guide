# Managing Elastic Beanstalk Application Environments<a name="create_deploy_Java.managingappenv"></a>


+ [Changing Environment Configuration Settings](#create_deploy_Java.managingappenv.env)
+ [Changing Environment Type](create_deploy_Java.managingappenv.envtype.md)
+ [Configuring EC2 Server Instances Using AWS Toolkit for Eclipse](create_deploy_Java.managingappenv.ec2.md)
+ [Configuring Elastic Load Balancing Using AWS Toolkit for Eclipse](create_deploy_Java.managingappenv.elb.md)
+ [Configuring Auto Scaling Using AWS Toolkit for Eclipse](create_deploy_Java.managingappenv.as.md)
+ [Configuring Notifications Using AWS Toolkit for Eclipse](create_deploy_Java.managingappenv.sns.md)
+ [Configuring Java Containers Using AWS Toolkit for Eclipse](create_deploy_Java.container.md)
+ [Setting System Properties with AWS Toolkit for Eclipse](#create_deploy_Java.managing.customenv.eclipse)

 With the AWS Toolkit for Eclipse, you can change the provisioning and configuration of the AWS resources that are used by your application environments\. For information on how to manage your application environments using the AWS Management Console, see [Managing Environments](using-features.managing.md)\. This section discusses the specific service settings you can edit in the AWS Toolkit for Eclipse as part of your application environment configuration\. For more about AWS Toolkit for Eclipse, see [AWS Toolkit for Eclipse Getting Started Guide](http://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/)\. 

## Changing Environment Configuration Settings<a name="create_deploy_Java.managingappenv.env"></a>

When you deploy your application, Elastic Beanstalk configures a number of AWS cloud computing services\. You can control how these individual services are configured using the AWS Toolkit for Eclipse\.

**To edit an application's environment settings**

1. If Eclipse isn't displaying the **AWS Explorer** view, in the menu click **Window** > **Show View** > **AWS Explorer**\. Expand the Elastic Beanstalk node and your application node\. 

1. In **AWS Explorer**, double\-click your Elastic Beanstalk environment\.

1. At the bottom of the pane, click the **Configuration** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-overview.png)

   You can now configure settings for the following:

   + EC2 server instances

   + Load balancer

   + Autoscaling

   + Notifications

   + Environment types

   + Environment properties

## Setting System Properties with AWS Toolkit for Eclipse<a name="create_deploy_Java.managing.customenv.eclipse"></a>

The following example sets the `JDBC_CONNECTION_STRING` system property in the AWS Toolkit for Eclipse\. After you set this properties, it becomes available to your Elastic Beanstalk application as system properties called `JDBC_CONNECTION_STRING`\.

**Note**  
 The AWS Toolkit for Eclipse does not yet support modifying environment configuration, including system properties, for environments in a VPC\. Unless you have an older account using EC2 Classic, you must use the AWS Management Console \(described in the next section\) or the [EB CLI](eb-cli3.md) 

**Note**  
Environment configuration settings can contain any printable ASCII character except the grave accent \(`, ASCII 96\) and cannot exceed 200 characters in length\.

 **To set system properties for your Elastic Beanstalk application** 

1. If Eclipse isn't displaying the **AWS Explorer** view, choose **Window**, **Show View**, **Other**\. Expand **AWS Toolkit** and then click **AWS Explorer**\.

1. In the **AWS Explorer** pane, expand **Elastic Beanstalk**, expand the node for your application, and then double\-click your Elastic Beanstalk environment\.

1. At the bottom of the pane for your environment, click the **Advanced** tab\.

1. Under **aws:elasticbeanstalk:application:environment**, click **JDBC\_CONNECTION\_STRING** and then type a connection string\. For example, the following JDBC connection string would connect to a MySQL database instance on port 3306 of localhost, with a user name of `me` and a password of `mypassword`:

    ` jdbc:mysql://localhost:3306/mydatabase?user=me&password=mypassword ` 

   This will be accessible to your Elastic Beanstalk application as a system property called `JDBC_CONNECTION_STRING`\.

1. Press **Ctrl\-C** on the keyboard or choose **File**, **Save** to save your changes to the environment configuration\. Changes are reflected in about one minute\. 