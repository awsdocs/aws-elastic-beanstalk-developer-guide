# Deploying to your environment<a name="create_deploy_NET.sdlc.create.edit"></a>

Now that you have tested your application, it is easy to edit and redeploy your application and see the results in moments\. 

 **To edit and redeploy your ASP\.NET web application ** 

1.  In **Solution Explorer**, right\-click your application, and then click **Republish to Environment <*your environment name*>**\. The **Re\-publish to AWS Elastic Beanstalk** wizard opens\.  
![\[Publish to beanstalk wizard 1\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-republish-beanstalk-sameenv.png)

1.  Review your deployment details and click **Deploy**\. 
**Note**  
If you want to change any of your settings, you can click **Cancel** and use the **Publish to AWS** wizard instead\. For instructions, see [Create an Elastic Beanstalk environment](dotnet-toolkit.md#create_deploy_NET.sdlc.deploy)\.

   Your updated ASP\.NET web project will be exported as a web deploy file with the new version label, uploaded to Amazon S3, and registered as a new application version with Elastic Beanstalk\. The Elastic Beanstalk deployment feature monitors your existing environment until it becomes available with the newly deployed code\. On the **env:<*environment name*>** tab, you will see the status of your environment\. 

You can also deploy an existing application to an existing environment if, for instance, you need to roll back to a previous application version\. 

**To deploy an application version to an existing environment**

1. Right\-click your Elastic Beanstalk application by expanding the Elastic Beanstalk node in **AWS Explorer**\. Select **View Status**\. 

1. In the **App: <*application name*>** tab, click **Versions**\.   
![\[Application versions\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-app-version.png)

1. Click the application version you want to deploy and click **Publish Version**\.

1.  In the **Publish Application Version** wizard, click **Next**\.  
![\[Publish application version wizard 1\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-republish-beanstalk2a.png)

1.  Review your deployment options, and click **Deploy**\.   
![\[Publish application version wizard 2\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-app-version-wizard3.png)

   Your ASP\.NET project will be exported as a web deploy file and uploaded to Amazon S3\. The Elastic Beanstalk deployment feature will monitor your environment until it becomes available with the newly deployed code\. On the **env:<*environment name*>** tab, you will see status for your environment\. 