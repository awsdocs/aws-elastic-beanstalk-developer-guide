# Getting started with Java on Elastic Beanstalk<a name="java-getstarted"></a>

To get started with Java applications on AWS Elastic Beanstalk, all you need is an application [source bundle](applications-sourcebundle.md) to upload as your first application version and to deploy to an environment\. When you create an environment, Elastic Beanstalk allocates all of the AWS resources needed to run a scalable web application\.

## Launching an environment with a sample Java application<a name="java-getstarted-samples"></a>

Elastic Beanstalk provides single page sample applications for each platform as well as more complex examples that show the use of additional AWS resources such as Amazon RDS and language or platform\-specific features and APIs\.

The single page samples are the same code that you get when you create an environment without supplying your own source code\. The more complex examples are hosted on GitHub and may need to be compiled or built prior to deploying to an Elastic Beanstalk environment\.


**Samples**  

|  Name  |  Supported versions  |  Environment type  |  Source  |  Description  | 
| --- | --- | --- | --- | --- | 
|  Tomcat \(single page\)  |  All *Tomcat with Corretto* platform branches  |  Web Server Worker  |   [tomcat\.zip](samples/tomcat.zip)   |  Tomcat web application with a single page \(`index.jsp`\) configured to be displayed at the website root\. For [worker environments](using-features-managing-env-tiers.md), this sample includes a `cron.yaml` file that configures a scheduled task that calls `scheduled.jsp` once per minute\. When `scheduled.jsp` is called, it writes to a log file at `/tmp/sample-app.log`\. Finally, a configuration file is included in `.ebextensions` that copies the logs from `/tmp/` to the locations read by Elastic Beanstalk when you request environment logs\. If you [enable X\-Ray integration](environment-configuration-debugging.md) on an environment running this sample, the application shows additional content regarding X\-Ray and provides an option to generate debug information that you can view in the X\-Ray console\.  | 
|  Corretto \(single page\)  |  Corretto 11 Corretto 8  |  Web Server  |  [corretto\.zip](samples/corretto.zip)  |  Corretto application with `Buildfile` and `Procfile` configuration files\. If you [enable X\-Ray integration](environment-configuration-debugging.md) on an environment running this sample, the application shows additional content regarding X\-Ray and provides an option to generate debug information that you can view in the X\-Ray console\.  | 
|  Scorekeep  | Java 8 | Web Server | [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-java-scorekeep) |  *Scorekeep* is a RESTful web API that uses the Spring framework to provide an interface for creating and managing users, sessions, and games\. The API is bundles with an Angular 1\.5 web app that consumes the API over HTTP\. The application uses features of the Java SE platform to download dependencies and build on\-instance, minimizing the size of the souce bundle\. The application also includes nginx configuration files that override the default configuration to serve the frontend web app statically on port 80 through the proxy, and route requests to paths under `/api` to the API running on `localhost:5000`\. Scorekeep also includes an `xray` branch that shows how to instrument a Java application for use with AWS X\-Ray\. It shows instrumentation of incoming HTTP requests with a servlet filter, automatic and manual AWS SDK client instrumentation, recorder configuration, and instrumentation of outgoing HTTP requests and SQL clients\. See the readme for instructions or use the [AWS X\-Ray getting started tutorial](https://docs.aws.amazon.com/xray/latest/devguide/xray-gettingstarted.html) to try the application with X\-Ray\.  | 
|  Does it Have Snakes?  | Tomcat 8 with Java 8 | Web Server | [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-tomcat-snakes) |  *Does it Have Snakes?* is a Tomcat web application that shows the use of Elastic Beanstalk configuration files, Amazon RDS, JDBC, PostgreSQL, Servlets, JSPs, Simple Tag Support, Tag Files, Log4J, Bootstrap, and Jackson\. The source code for this project includes a minimal build script that compiles the servlets and models into class files and packages the required files into a Web Archive that you can deploy to an Elastic Beanstalk environment\. See the readme file in the project repository for full instructions\.  | 
| Locust Load Generator | Java 8 | Web Server | [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-locustio-sample) |  Web application that you can use to load test another web application running in a different Elastic Beanstalk environment\. Shows the use of `Buildfile` and `Procfile` files, DynamoDB, and [Locust](http://locust.io/), an open source load testing tool\.  | 

Download any of the sample applications and deploy it to Elastic Beanstalk by following these steps:

**To launch an environment with a sample application \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose an existing application's name in the list or [create one](applications.md)\.

1. On the application overview page, choose **Create a new environment**\.  
![\[The application overview page with a list of application environments on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-mgmt-environments.png)

1. Choose the **Web server environment** or **Worker environment** [environment tier](concepts.md#concepts-tier)\. You can't change an environment's tier after creation\.
**Note**  
The [\.NET on Windows Server platform](create_deploy_NET.md) doesn't support the worker environment tier\.  
![\[The Select environment tier page on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-choosetier.png)

1. For **Platform**, select the platform and platform branch that match the language your application uses\.
**Note**  
Elastic Beanstalk supports multiple [versions](concepts.platforms.md) for most of the platforms that are listed\. By default, the console selects the recommended version for the platform and platform branch you choose\. If your application requires a different version, you can select it here, or choose **Configure more options**, as described in step 7\. For information about supported platform versions, see [Elastic Beanstalk supported platforms](concepts.platforms.md)\.

1. For **Application code**, choose **Sample application**\.

1. To further customize your environment, choose **Configure more options**\. You can set the following options only during environment creation:
   + Environment name
   + Domain name
   + Platform version
   + VPC
   + Tier

   You can change the following settings after environment creation, but they require new instances or other resources to be provisioned and can take a long time to apply:
   + Instance type, root volume, key pair, and AWS Identity and Access Management \(IAM\) role
   + Internal Amazon RDS database
   + Load balancer

   For details on all available settings, see [The create new environment wizard](environments-create-wizard.md)\.

1. Choose **Create environment**\.

## Next steps<a name="java-getstarted-next"></a>

After you have an environment running an application, you can [deploy a new version](using-features.deploy-existing-version.md) of the application or a completely different application at any time\. Deploying a new application version is very quick because it doesn't require provisioning or restarting EC2 instances\.

After you've deployed a sample application or two and are ready to start developing and running Java applications locally, see [the next section](java-development-environment.md) to set up a Java development environment with all of the tools and libraries that you will need\.