# Step 3: Deploy a new version of your application<a name="GettingStarted.DeployApp"></a>

Periodically, you might need to deploy a new version of your application\. You can deploy a new version at any time, as long as no other update operations are in progress on your environment\.

The application version that you started this tutorial with is called **Sample Application**\.

**To update your application version**

1. Download the sample application that matches your environment's platform\. Use one of the following applications\.
   + **Docker** – [docker\.zip](samples/docker.zip)
   + **Multicontainer Docker** – [docker\-multicontainer\-v2\.zip](samples/docker-multicontainer-v2.zip)
   + **Preconfigured Docker \(Glassfish\)** – [docker\-glassfish\-v1\.zip](samples/docker-glassfish-v1.zip)
   + **Go** – [go\.zip](samples/go.zip)
   + **Corretto** – [corretto\.zip](samples/corretto.zip)
   + **Tomcat** – [tomcat\.zip](samples/tomcat.zip)
   + **\.NET Core on Linux** – [dotnet\-core\-linux\.zip](samples/dotnet-core-linux.zip)
   + **\.NET** – [dotnet\-asp\-v1\.zip](samples/dotnet-asp-v1.zip)
   + **Node\.js** – [nodejs\.zip](samples/nodejs.zip) 
   + **PHP** – [php\.zip](samples/php.zip)
   + **Python** – [python\.zip](samples/python.zip)
   + **Ruby** – [ruby\.zip](samples/ruby.zip)

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Choose **Choose file**, and then upload the sample application source bundle that you downloaded\.  
![\[Upload and deploy dialog.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-version-upload.png)

   The console automatically fills in the **Version label** with a new unique label\. If you type in your own version label, ensure that it's unique\.

1. Choose **Deploy**\.

 While Elastic Beanstalk deploys your file to your Amazon EC2 instances, you can view the deployment status on the environment's overview\. While the application version is updated, the **Environment Health** status is gray\. When the deployment is complete, Elastic Beanstalk performs an application health check\. When the application responds to the health check, it's considered healthy and the status returns to green\. The environment overview shows the new **Running Version**—the name you provided as the **Version label**\.

Elastic Beanstalk also uploads your new application version and adds it to the table of application versions\. To view the table, choose **Application versions** under **getting\-started\-app** on the navigation pane\.