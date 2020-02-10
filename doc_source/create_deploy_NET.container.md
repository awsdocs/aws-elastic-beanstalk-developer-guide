# Configuring \.NET containers using the AWS toolkit for Visual Studio<a name="create_deploy_NET.container"></a>

 The **Container/\.NET Options** panel lets you fine\-tune the behavior of your Amazon EC2 instances and enable or disable Amazon S3 log rotation\. You can use the AWS Toolkit for Visual Studio to configure your container information\.

**Note**  
You can modify your configuration settings with zero downtime by swapping the CNAME for your environments\. For more information, see [Blue/Green deployments with Elastic Beanstalk](using-features.CNAMESwap.md)\.

If you want to, you can extend the number of parameters\. For information about extending parameters, see [Option settings](ebextensions-optionsettings.md)\.

**To access the Container/\.NET options panel for your Elastic Beanstalk application**

1. In AWS Toolkit for Visual Studio, expand the Elastic Beanstalk node and your application node\. 

1. In **AWS Explorer**, double\-click your Elastic Beanstalk environment\.

1. At the bottom of the **Overview** pane, click the **Configuration** tab\.

1. Under **Container**, you can configure container options\.   
![\[Elastic Beanstalk container panel\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-container.png)

## \.NET container options<a name="create_deploy_NET.container.vs.options"></a>

You can choose the version of \.NET Framework for your application\. Choose either 2\.0 or 4\.0 for **Target runtime**\. Select **Enable 32\-bit Applications** if you want to enable 32\-bit applications\.

## Application settings<a name="create_deploy_NET.container.vs.options.envprop"></a>

The **Application Settings** section lets you specify environment variables that you can read from your application code\. 

![\[Elastic Beanstalk container panel\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-container-envproperties.png)