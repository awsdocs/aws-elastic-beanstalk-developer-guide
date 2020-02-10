# Deploying Elastic Beanstalk applications in \.NET using the deployment tool<a name="deploy_NET_standalone_tool"></a>

The AWS Toolkit for Visual Studio includes a deployment tool, a command line tool that provides the same functionality as the deployment wizard in the AWS Toolkit\. You can use the deployment tool in your build pipeline or in other scripts to automate deployments to Elastic Beanstalk\.

The deployment tool supports both initial deployments and redeployments\. If you previously deployed your application using the deployment tool, you can redeploy using the deployment wizard within Visual Studio\. Similarly, if you have deployed using the wizard, you can redeploy using the deployment tool\.

**Note**  
The deployment tool does not apply [recommended values](command-options.md#configuration-options-recommendedvalues) for configuration options like the console or EB CLI\. Use [configuration files](ebextensions.md) to ensure that any settings that you need are configured when you launch your environment\.

This chapter walks you through deploying a sample \.NET application to Elastic Beanstalk using the deployment tool, and then redeploying the application using an incremental deployment\. For a more in\-depth discussion about the deployment tool, including the parameter options, see [Deployment Tool](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv-deploy-beanstalk.html)\.

## Prerequisites<a name="deploy_NET_standalone_tool.prereq"></a>

To use the deployment tool, you need to install the AWS Toolkit for Visual Studio\. For information on prerequisites and installation instructions, see [AWS Toolkit for Microsoft Visual Studio](https://aws.amazon.com/visualstudio/)\.

The deployment tool is typically installed in one of the following directories on Windows:


****  

| 32\-bit | 64\-bit | 
| --- | --- | 
|  C:\\Program Files\\AWS Tools\\Deployment Tool\\awsdeploy\.exe  |  C:\\Program Files \(x86\)\\AWS Tools\\Deployment Tool\\awsdeploy\.exe  | 

## Deploy to Elastic Beanstalk<a name="deploy_NET_standalone_tool.deploy"></a>

To deploy the sample application to Elastic Beanstalk using the deployment tool, you first need to modify the `ElasticBeanstalkDeploymentSample.txt` configuration file, which is provided in the `Samples` directory\. This configuration file contains the information necessary to deploy your application, including the application name, application version, environment name, and your AWS access credentials\. After modifying the configuration file, you then use the command line to deploy the sample application\. Your web deploy file is uploaded to Amazon S3 and registered as a new application version with Elastic Beanstalk\. It will take a few minutes to deploy your application\. Once the environment is healthy, the deployment tool outputs a URL for the running application\. 

**To deploy a \.NET application to Elastic Beanstalk**

1. From the `Samples` subdirectory where the deployment tool is installed, open `ElasticBeanstalkDeploymentSample.txt` and enter your AWS access key and AWS secret key as in the following example\.

   ```
   ### AWS Access Key and Secret Key used to create and deploy the application instance
   AWSAccessKey = AKIAIOSFODNN7EXAMPLE
   AWSSecretKey = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```
**Note**  
For API access, you need an access key ID and secret access key\. Use IAM user access keys instead of AWS account root user access keys\. For more information about creating access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\. 

1. At the command line prompt, type the following:

   ```
   C:\Program Files (x86)\AWS Tools\Deployment Tool>awsdeploy.exe /w Samples\ElasticBeanstalkDeploymentSample.txt
   ```

   It takes a few minutes to deploy your application\. If the deployment succeeds, you will see the message, `Application deployment completed; environment health is Green`\.
**Note**  
If you receive the following error, the CNAME already exists\.   

   ```
   [Error]: Deployment to AWS Elastic Beanstalk failed with exception: DNS name (MyAppEnv.elasticbeanstalk.com) is not available.
   ```
Because a CNAME must be unique, you need to change `Environment.CNAME` in `ElasticBeanstalkDeploymentSample.txt`\. 

1. In your web browser, navigate to the URL of your running application\. The URL will be in the form <CNAME\.elasticbeanstalk\.com> \(e\.g\., **MyAppEnv\.elasticbeanstalk\.com**\)\.