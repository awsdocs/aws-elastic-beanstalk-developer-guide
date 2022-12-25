# Deploying Elastic Beanstalk applications from Docker containers<a name="create_deploy_docker"></a>

Elastic Beanstalk supports the deployment of web applications from Docker containers\. With Docker containers, you can define your own runtime environment\. You can also choose your own platform, programming language, and any application dependencies \(such as package managers or tools\), which typically aren't supported by other platforms\. Docker containers are self contained and include all the configuration information and software that your web application requires to run\. All environment variables that are defined in the Elastic Beanstalk console are passed to the containers\. 

By using Docker with Elastic Beanstalk, you have an infrastructure that handles all the details of capacity provisioning, load balancing, scaling, and application health monitoring\. You can easily manage your web application in an environment that supports the range of services that are integrated with Elastic Beanstalk\. These environments include but not limited to [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html), [RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html), and [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html)\. For more information about Docker, including how to install it, what software it requires, and how to use Docker images to launch Docker containers, see [Docker: the Linux container engine](http://www.docker.io)\.



The topics in this chapter assume that you have some some knowledge of Elastic Beanstalk environments\. If you haven't used Elastic Beanstalk before, try the [getting started tutorial](GettingStarted.md) to learn the basics\.

## Docker platform family<a name="docker-platform"></a>

The Docker platform family for Elastic Beanstalk includes several platforms\. The Docker platform that runs on Amazon Linux 2 offers the most benefits, like long\-term support\. The sections that follow detail the Docker platforms that Elastic Beanstalk offers and recommended migration paths to Amazon Linux 2\.

For more information about supported platform versions for each Docker platform, see the [Supported Platforms](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.docker) page in the *AWS Elastic Beanstalk Platforms* document\.

### The Docker platform<a name="docker-platform-single"></a>

 Elastic Beanstalk can deploy a Docker image and source code to EC2 instances running the Elastic Beanstalk Docker platform\. The platform offers multi\-container \(and single\-container\) support\. You can also leverage the Docker Compose tool on the Docker platform to simplify your application configuration, testing and deployment\. 

This Amazon Linux 2 Docker platform offers the following benefits:
+  *Long\-term support\.* The Docker on Amazon Linux 2 platform has long\-term support, offering security and feature updates\. 
+  *Docker Compose features\.* This platform will allow you to leverage the features provided by the Docker Compose tool to define and run multiple containers\. You can include the *docker\-compose\.yml* file to deploy to Elastic Beanstalk\. 
+  *Use of application images from public or private repositories\.* Elastic Beanstalk invokes the Docker Compose command line interface, processing the *docker\-compose\.yml* file to pull the application images and run them as containerized applications\. 
+  *Build container images during deployment\.* You don’t need to pre\-build your application images before deploying them to run as containers\. During deployment you can build the container images from scratch by specifying dependencies in the Dockerfile\. 

For more information on samples and help getting started with a Docker environment, see [Using the Docker platform](docker.md)\. For more information on the container definition formats and their use, see [Docker configuration](single-container-docker-configuration.md)\.

The following sections are relevant to Elastic Beanstalk Docker environments that uses the earlier Amazon Linux AMI platform version \(precedes Amazon Linux 2\)\.

### Docker \(Amazon Linux AMI\)<a name="docker-platform-single"></a>

The Amazon Linux AMI\-based Docker platform can be used to deploy a Docker image \(described in a Dockerfile or `Dockerrun.aws.json` definition\) and source code to EC2 instances that are running in an Elastic Beanstalk environment\. This Docker platform runs only one container for each instance\.

For samples and help getting started with a Docker environment, see [Using the Docker platform](docker.md)\. For more information about the container definition formats and their use, see [Docker configuration](single-container-docker-configuration.md)\.

### Multicontainer Docker \(Amazon Linux AMI\)<a name="docker-platform-multi"></a>

**Note**  
 This platform only supports the Amazon Linux AMI operating system \(the version that precedes Amazon Linux 2\)\. The [Docker](docker.md) platform provides Multicontainer Docker functionality with Amazon Linux 2\.

The other generic platform, Multicontainer Docker, uses the Amazon Elastic Container Service \(Amazon ECS\) to coordinate a deployment of multiple Docker containers to an Amazon ECS cluster in an Elastic Beanstalk environment\. The instances in the environment each run the same set of containers, which are defined in a `Dockerrun.aws.json` file\. If your Elastic Beanstalk environment uses an Amazon Linux AMI platform version \(precedes Amazon Linux 2\), use the multicontainer platform to deploy multiple Docker containers to each instance\.

For more information about the Multicontainer Docker platform and its use, see [Using the Multicontainer Docker platform \(Amazon Linux AMI\)](create_deploy_docker_ecs.md)\. The [Multicontainer Docker configuration](create_deploy_docker_v2config.md) topic details version 2 of the `Dockerrun.aws.json` format, which is similar to but not compatible with the version used with the Docker platform\. There is also a [tutorial](create_deploy_docker_ecstutorial.md) that guides you through a deployment of a multicontainer environment from scratch\. The environment that is described runs a PHP website with an NGINX proxy running in front of it in a separate container\.

### Preconfigured Docker containers<a name="docker-platform-preconfigured"></a>

In addition to the two generic Docker platforms, there are several *preconfigured* Docker platform branches that you can use to run your application in one of several popular software stacks like *Java with GlassFish* or *Python with uWSGI*\. Use a preconfigured container if it matches the software that your application uses\.

**Note**  
All the Preconfigured Docker platform branches use the Amazon Linux AMI operating system \(version that precedes Amazon Linux 2\)\. To migrate your GlassFish application to Amazon Linux 2, use the generic Docker platform and deploy GlassFish and your application code to an Amazon Linux 2 Docker image\. For more information about this, see [Deploying a GlassFish application to the Docker platform: a migration path to Amazon Linux 2](docker-glassfish-tutorial.md)\.

For more information, see [Preconfigured Docker containers ](create_deploy_dockerpreconfig.md)\.