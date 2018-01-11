# AWS Elastic Beanstalk Concepts<a name="concepts"></a>

AWS Elastic Beanstalk lets you manage all of the resources that run your *application* as *environments*\. Let's take a closer look at what these terms mean\.

## Application<a name="concepts-application"></a>

An Elastic Beanstalk *application* is a logical collection of Elastic Beanstalk components, including *environments*, *versions*, and *environment configurations*\. In Elastic Beanstalk an application is conceptually similar to a folder\.

## Application Version<a name="concepts-version"></a>

In Elastic Beanstalk, an *application version* refers to a specific, labeled iteration of deployable code for a web application\. An application version points to an Amazon Simple Storage Service \(Amazon S3\) object that contains the deployable code such as a Java WAR file\. An application version is part of an application\. Applications can have many versions and each application version is unique\. In a running environment, you can deploy any application version you already uploaded to the application or you can upload and immediately deploy a new application version\. You might upload multiple application versions to test differences between one version of your web application and another\.

## Environment<a name="concepts-environment"></a>

An *environment* is a version that is deployed onto AWS resources\. Each environment runs only a single application version at a time, however you can run the same version or different versions in many environments at the same time\. When you create an environment, Elastic Beanstalk provisions the resources needed to run the application version you specified\.

## Environment Tier<a name="concepts-tier"></a>

When you launch an Elastic Beanstalk environment, you first choose an environment tier\. The environment tier that you choose determines whether Elastic Beanstalk provisions resources to support an application that handles HTTP requests or an application that pulls tasks from a queue\. An application that serves HTTP requests runs in a web server environment\. An environment that pulls tasks from an Amazon Simple Queue Service queue runs in a worker environment\.

## Environment Configuration<a name="concepts-environmentconfig"></a>

 An *environment configuration* identifies a collection of parameters and settings that define how an environment and its associated resources behave\. When you update an environment’s configuration settings, Elastic Beanstalk automatically applies the changes to existing resources or deletes and deploys new resources \(depending on the type of change\)\.

## Configuration Template<a name="concepts-configuration"></a>

A *configuration template* is a starting point for creating unique environment configurations\. Configuration templates can be created or modified by using the Elastic Beanstalk command line utilities or API\.