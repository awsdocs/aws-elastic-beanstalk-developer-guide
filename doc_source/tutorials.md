# Tutorials and Samples<a name="tutorials"></a>

Language and framework specific tutorials are spread throughout the AWS Elastic Beanstalk Developer Guide\. New and updated tutorials are added to this list as they are published\. The most recent updates are shown first\.

These tutorials are targeted at intermediate users and may not contain instructions for basic steps such as signing up for AWS\. If this is your first time using AWS or Elastic Beanstalk, check out the Getting Started walkthrough to get your first Elastic Beanstalk environment up and running\.

+ **PHP and Drupal HA Configuration** \- 

+ **PHP and WordPress HA Configuration** \- 

+ **Node\.js with DynamoDB HA Configuration** \- 

+ **ASP\.NET Core** \- 

+ **PHP 5\.6 and MySQL HA Configuration** \- 

+ **PHP 5\.6 and Laravel 5\.2** \- 

+ **PHP 5\.6 and CakePHP 3\.2** \- 

+ **Python and Flask 0\.10** \- 

+ **Python and Django 1\.9** \- 

+ **Node\.js and Express 4** \- 

+ **Docker, PHP and nginx** \- 

+ **Ruby on Rails** \- 

+ **Ruby and Sinatra** \- 

+ **\.NET Framework \(IIS and ASP\.NET\)** \- 

You can download the sample applications used by Elastic Beanstalk when you create an environment without providing a source bundle with the following links:

+ **Single Container Docker** – [docker\-singlecontainer\-v1\.zip](samples/docker-singlecontainer-v1.zip)

+ **Multicontainer Docker** – [docker\-multicontainer\-v2\.zip](samples/docker-multicontainer-v2.zip)

+ **Preconfigured Docker \(Glassfish\)** – [docker\-glassfish\-v1\.zip](samples/docker-glassfish-v1.zip)

+ **Preconfigured Docker \(Python 3\)** – [docker\-python\-v1\.zip](samples/docker-python-v1.zip)

+ **Preconfigured Docker \(Go\)** – [docker\-golang\-v1\.zip](samples/docker-golang-v1.zip)

+ **Go** – [go\-v1\.zip](samples/go-v1.zip)

+ **Java SE** – [java\-se\-jetty\-gradle\-v3\.zip](samples/java-se-jetty-gradle-v3.zip)

+ **Tomcat** – [java\-tomcat\-v3\.zip](samples/java-tomcat-v3.zip)

+ **\.NET** – [dotnet\-asp\-v1\.zip](samples/dotnet-asp-v1.zip)

+ **Node\.js** – [nodejs\-v1\.zip](samples/nodejs-v1.zip) 

+ **PHP** – [php\-v1\.zip](samples/php-v1.zip)

+ **Python** – [python\-v1\.zip](samples/python-v1.zip)

+ **Ruby \(Passenger Standalone\)** – [ruby\-passenger\-v2\.zip](samples/ruby-passenger-v2.zip)

+ **Ruby \(Puma\)** – [ruby\-puma\-v2\.zip](samples/ruby-puma-v2.zip)

More involved sample applications that show the use of additional web frameworks, libraries and tools are available as open source projects on GitHub:

+ **[Load Balanced WordPress](https://github.com/awslabs/eb-php-wordpress)** \(tutorial\) – Configuration files for installing WordPress securely and running it in a load balanced AWS Elastic Beanstalk environment\.

+ **[Load Balanced Drupal](https://github.com/awslabs/eb-php-drupal)** \(tutorial\) – Configuration files and instructions for installing Drupal securely and running it in a load balanced AWS Elastic Beanstalk environment\. 

+ **[Scorekeep](https://github.com/awslabs/eb-java-scorekeep)** \- RESTful web API that uses the Spring framework and the AWS SDK for Java to provide an interface for creating and managing users, sessions, and games\. The API is bundled with an Angular 1\.5 web app that consumes the API over HTTP\. Includes branches that show integration with Amazon Cognito, AWS X\-Ray, and Amazon Relational Database Service\.

  The application uses features of the Java SE platform to download dependencies and build on\-instance, minimizing the size of the souce bundle\. The application also includes nginx configuration files that override the default configuration to serve the frontend web app statically on port 80 through the proxy, and route requests to paths under `/api` to the API running on `localhost:5000`\.

+ **[Does it Have Snakes?](https://github.com/awslabs/eb-tomcat-snakes)** \- Tomcat application that shows the use of RDS in a Java EE web application in AWS Elastic Beanstalk\. The project shows the use of Servlets, JSPs, Simple Tag Support, Tag Files, JDBC, SQL, Log4J, Bootstrap, Jackson, and Elastic Beanstalk configuration files\.

+ **[Locust Load Generator ](https://github.com/awslabs/eb-locustio-sample)** \- This project shows the use of Java SE platform features to install and run [Locust](http://locust.io/), a load generating tool written in Python\. The project includes configuration files that install and configure Locust, a build script that configures a DynamoDB table, and a Procfile that runs Locust\.

+ **[Share Your Thoughts](https://github.com/awslabs/eb-demo-php-simple-app)** \(tutorial\) \- PHP application that shows the use of MySQL on Amazon RDS, Composer, and configuration files\.

+ **[A New Startup](https://github.com/awslabs/eb-node-express-sample)** \(tutorial\) \- Node\.js sample application that shows the use of DynamoDB, the AWS SDK for JavaScript in Node\.js, npm package management, and configuration files\.