# Deploying Elastic Beanstalk applications from Docker containers<a name="create_deploy_docker"></a>

Elastic Beanstalk supports the deployment of web applications from Docker containers\. With Docker containers, you can define your own runtime environment\. You can choose your own platform, programming language, and any application dependencies \(such as package managers or tools\), that aren't supported by other platforms\. Docker containers are self\-contained and include all the configuration information and software your web application requires to run\. All environment variables defined in the Elastic Beanstalk console are passed to the containers\. 

By using Docker with Elastic Beanstalk, you have an infrastructure that automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring\. You can manage your web application in an environment that supports the range of services that are integrated with Elastic Beanstalk, including but not limited to [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html), [RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html), and [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html)\. For more information about Docker, including how to install it, what software it requires, and how to use Docker images to launch Docker containers, go to [Docker: the Linux container engine](http://www.docker.io)\.

**Note**  
If a Docker container running in an Elastic Beanstalk environment crashes or is killed for any reason, Elastic Beanstalk restarts it automatically\.

The topics in this chapter assume some knowledge of Elastic Beanstalk environments\. If you haven't used Elastic Beanstalk before, try the [getting started tutorial](GettingStarted.md) to learn the basics\.

## Docker platform family<a name="docker-platform"></a>

The Docker platform family for Elastic Beanstalk has two generic platforms \(*Docker* and *Multicontainer Docker*\), and several preconfigured containers\.

See the [Supported Platforms](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.docker) page in the *AWS Elastic Beanstalk Platforms* document for details on the currently supported platform versions of each Docker platform\.

### Docker<a name="docker-platform-single"></a>

The Docker platform can be used to deploy a Docker image \(described in a Dockerfile or Dockerrun\.aws\.json definition\) and source code to EC2 instances running in an Elastic Beanstalk environment\. Use the Docker platform when you only need to run one container per instance\.

For samples and help getting started with a Docker environment, see [Using the Docker platform](single-container-docker.md)\. For detailed information on the container definition formats and their use, see [Docker configuration](single-container-docker-configuration.md)\.

### Multicontainer Docker<a name="docker-platform-multi"></a>

The other generic platform, Multicontainer Docker, uses the Amazon Elastic Container Service to coordinate a deployment of multiple Docker containers to an Amazon ECS cluster in an Elastic Beanstalk environment\. The instances in the environment each run the same set of containers, which are defined in a `Dockerrun.aws.json` file\. Use the multicontainer platform when you need to deploy multiple Docker containers to each instance\.

For more details on the Multicontainer Docker platform and its use, see [Multicontainer Docker environments](create_deploy_docker_ecs.md)\. The [Multicontainer Docker configuration](create_deploy_docker_v2config.md) topic details version 2 of the Dockerrun\.aws\.json format, which is similar to but not compatible with the version used with the Docker platform\. There is also a [tutorial](create_deploy_docker_ecstutorial.md) available that guides you through a from scratch deployment of a multicontainer environment running a PHP website with an nginx proxy running in front of it in a separate container\.

### Preconfigured Docker containers<a name="docker-platform-preconfigured"></a>

In addition to the two generic Docker platforms, there are several *preconfigured* Docker platform branches that you can use to run your application in a popular software stack such as *Java with GlassFish* or *Python with uWSGI*\. Use a preconfigured container if it matches the software used by your application\.

**Note**  
All the Preconfigured Docker platform branches use the Amazon Linux AMI operating system \(preceding Amazon Linux 2\)\. To migrate your GlassFish application to Amazon Linux 2, use the generic Docker platform and deploy GlassFish and your application code to an Amazon Linux 2 Docker image\. For details, see [Deploying a GlassFish application to the Docker platform](docker-glassfish-tutorial.md)\.

For more information, see [Preconfigured Docker containers](create_deploy_dockerpreconfig.md)\.