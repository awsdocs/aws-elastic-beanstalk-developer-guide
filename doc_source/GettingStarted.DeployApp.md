# Step 3: Deploy a new version of your application<a name="GettingStarted.DeployApp"></a>

Periodically, you might need to deploy a new version of your application\. You can deploy a new version at any time, as long as no other update operations are in progress on your environment\.

The application version that you started this tutorial with is called **Sample Application**\.

**To update your application version**

1. Download the sample application that matches your environment's platform\. Use one of the following applications\.
   + **Single Container Docker** – [docker\-singlecontainer\-v1\.zip](samples/docker-singlecontainer-v1.zip)
   + **Multicontainer Docker** – [docker\-multicontainer\-v2\.zip](samples/docker-multicontainer-v2.zip)
   + **Preconfigured Docker \(Glassfish\)** – [docker\-glassfish\-v1\.zip](samples/docker-glassfish-v1.zip)
   + **Go** – [go\-v1\.zip](samples/go-v1.zip)
   + **Java SE** – [java\-se\-jetty\-gradle\-v3\.zip](samples/java-se-jetty-gradle-v3.zip)
   + **Tomcat \(default\)** – [java\-tomcat\-v3\.zip](samples/java-tomcat-v3.zip)
   + **Tomcat 7** – [java7\-tomcat7\.zip](samples/java7-tomcat7.zip)
   + **\.NET** – [dotnet\-asp\-v1\.zip](samples/dotnet-asp-v1.zip)
   + **Node\.js** – [nodejs\-v1\.zip](samples/nodejs-v1.zip) 
   + **PHP** – [php\-v1\.zip](samples/php-v1.zip)
   + **Python** – [python\-v1\.zip](samples/python-v1.zip)
   + **Ruby \(Passenger Standalone\)** – [ruby\-passenger\-v3\.zip](samples/ruby-passenger-v3.zip)
   + **Ruby \(Puma\)** – [ruby\-puma\-v3\.zip](samples/ruby-puma-v3.zip)

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and then, in the regions drop\-down list, select your region\.

1. In the navigation pane, choose **Environments**, and then choose your environment's name on the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Choose **Choose file**, and then upload the sample application source bundle that you downloaded\.  
![\[Upload and deploy dialog.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-version-upload.png)

   The console automatically fills in the **Version label** with a new unique label\. If you type in your own version label, ensure that it's unique\.

1. Choose **Deploy**\.

 While Elastic Beanstalk deploys your file to your Amazon EC2 instances, you can view the deployment status on the environment's overview\. While the application version is updated, the **Environment Health** status is gray\. When the deployment is complete, Elastic Beanstalk performs an application health check\. When the application responds to the health check, it's considered healthy and the status returns to green\. The environment overview shows the new **Running Version**—the name you provided as the **Version label**\.

Elastic Beanstalk also uploads your new application version and adds it to the table of application versions\. To view the table, choose **Application versions** under **getting\-started\-app** on the navigation pane\.