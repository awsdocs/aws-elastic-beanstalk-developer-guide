# Getting Started with Java on Elastic Beanstalk<a name="java-getstarted"></a>

To get started with Java applications on AWS Elastic Beanstalk, all you need is an application [source bundle](applications-sourcebundle.md) to upload as your first application version and to deploy to an environment\. When you create an environment, Elastic Beanstalk allocates all of the AWS resources needed to run a scalable web application\.

## Launching an Environment with a Sample Java Application<a name="java-getstarted-samples"></a>

Elastic Beanstalk provides single page sample applications for each platform as well as more complex examples that show the use of additional AWS resources such as Amazon RDS and language or platform\-specific features and APIs\.

The single page samples are the same code that you get when you create an environment without supplying your own source code\. The more complex examples are hosted on GitHub and may need to be compiled or built prior to deploying to an Elastic Beanstalk environment\.


**Samples**  

|  Name  |  Supported Configurations  |  Environment Type  |  Source  |  Description  | 
| --- | --- | --- | --- | --- | 
|  Tomcat Default  |  Tomcat 8 with Java 8 Tomcat 7 with Java 7 Tomcat 7 with Java 6  |  Web Server Worker  |   [java\-tomcat\-v3\.zip](samples/java-tomcat-v3.zip)   |  Tomcat web application with a single page \(`index.jsp`\) configured to be displayed at the website root\. For [worker environments](using-features-managing-env-tiers.md), this sample includes a `cron.yaml` file that configures a scheduled task that calls `scheduled.jsp` once per minute\. When `scheduled.jsp` is called, it writes to a log file at `/tmp/sample-app.log`\. Finally, a configuration file is included in `.ebextensions` that copies the logs from `/tmp/` to the locations read by Elastic Beanstalk when you request environment logs\. If you [enable X\-Ray integration](environment-configuration-debugging.md) on an environment running this sample, the application shows additional content regarding X\-Ray and provides an option to generate debug information that you can view in the X\-Ray console\.  | 
|  Java SE Default  |  Java 8 Java 7  |  Web Server  |  [java\-se\-jetty\-gradle\-v3\.zip](samples/java-se-jetty-gradle-v3.zip)  |  Jetty SE application with `Buildfile` and `Procfile` configuration files\. The `Buildfile` in this sample runs a Gradle command to build the application source on\-instance\. If you [enable X\-Ray integration](environment-configuration-debugging.md) on an environment running this sample, the application shows additional content regarding X\-Ray and provides an option to generate debug information that you can view in the X\-Ray console\.  | 
|  Scorekeep  | Java 8 | Web Server | [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-java-scorekeep) |  *Scorekeep* is a RESTful web API that uses the Spring framework to provide an interface for creating and managing users, sessions, and games\. The API is bundles with an Angular 1\.5 web app that consumes the API over HTTP\. The application uses features of the Java SE platform to download dependencies and build on\-instance, minimizing the size of the souce bundle\. The application also includes nginx configuration files that override the default configuration to serve the frontend web app statically on port 80 through the proxy, and route requests to paths under `/api` to the API running on `localhost:5000`\. Scorekeep also includes an `xray` branch that shows how to instrument a Java application for use with AWS X\-Ray\. It shows instrumentation of incoming HTTP requests with a servlet filter, automatic and manual AWS SDK client instrumentation, recorder configuration, and instrumentation of outgoing HTTP requests and SQL clients\. See the readme for instructions or use the [AWS X\-Ray getting started tutorial](https://docs.aws.amazon.com/xray/latest/devguide/xray-gettingstarted.html) to try the application with X\-Ray\.  | 
|  Does it Have Snakes?  | Tomcat 8 with Java 8 | Web Server | [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-tomcat-snakes) |  *Does it Have Snakes?* is a Tomcat web application that shows the use of Elastic Beanstalk configuration files, Amazon RDS, JDBC, PostgreSQL, Servlets, JSPs, Simple Tag Support, Tag Files, Log4J, Bootstrap, and Jackson\. The source code for this project includes a minimal build script that compiles the servlets and models into class files and packages the required files into a Web Archive that you can deploy to an Elastic Beanstalk environment\. See the readme file in the project repository for full instructions\.  | 
| Locust Load Generator | Java 8 | Web Server | [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-locustio-sample) |  Web application that you can use to load test another web application running in a different Elastic Beanstalk environment\. Shows the use of `Buildfile` and `Procfile` files, DynamoDB, and [Locust](http://locust.io/), an open source load testing tool\.  | 

Download any of the sample applications and deploy it to Elastic Beanstalk by following these steps:

**To launch an environment with a sample application \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application or [create a new one](applications.md)\.

1. From the **Actions** menu in the upper right corner, choose **Create environment**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/application-actions-createnewenvironment.png)

1. Choose either the **Web server environment** or **Worker environment** [environment tier](concepts.md#concepts-tier)\. You cannot change an environment's tier after creation\.
**Note**  
The [\.NET on Windows Server platform](create_deploy_NET.md) doesn't support the worker environment tier\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-choosetier.png)

1. Choose a **Platform** that matches the language used by your application\.
**Note**  
Elastic Beanstalk supports multiple [versions](concepts.platforms.md) for most platforms listed\. By default, the console selects the latest version of the language, web container, or framework [supported by Elastic Beanstalk](concepts.platforms.md)\. If your application requires an older version, choose **Configure more options**, as described below\.

1. For **App code**, choose **Sample application**\.

1. If you want to further customize your environment, choose **Configure more options**\. The following options can be set only during environment creation:
   + Environment name
   + Domain name
   + Platform version \(configuration\)
   + VPC
   + Tier

   The following settings can be changed after environment creation, but require new instances or other resources to be provisioned and can take a long time to apply:
   + Instance type, root volume, key pair, and IAM role
   + Internal Amazon RDS database
   + Load balancer

   For details on all available settings, see [The Create New Environment Wizard](environments-create-wizard.md)\.

1. Choose **Create environment**\.

## Next Steps<a name="java-getstarted-next"></a>

After you have an environment running an application, you can [deploy a new version](using-features.deploy-existing-version.md) of the application or a completely different application at any time\. Deploying a new application version is very quick because it doesn't require provisioning or restarting EC2 instances\.

After you've deployed a sample application or two and are ready to start developing and running Java applications locally, see [the next section](java-development-environment.md) to set up a Java development environment with all of the tools and libraries that you will need\.