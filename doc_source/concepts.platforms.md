# Elastic Beanstalk Supported Platforms<a name="concepts.platforms"></a>

AWS Elastic Beanstalk provides platforms for programming languages \(Java, PHP, Python, Ruby, Go\), web containers \(Tomcat, Passenger, Puma\) and Docker containers, with multiple configurations of each\.

Elastic Beanstalk provisions the resources needed to run your application, including one or more Amazon EC2 instances\. The software stack running on the Amazon EC2 instances depends on the configuration\. In a configuration name, the version number refers to the version of the platform configuration\.

You can use the solution stack name listed under the configuration name to launch an environment with the [EB CLI](eb-cli3.md), [Elastic Beanstalk API](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/), or [AWS CLI](https://aws.amazon.com/cli/)\. You can also retrieve solution stack names from the service with the [ListAvailableSolutionStacks](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html) API \([aws elasticbeanstalk list\-available\-solution\-stacks](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-available-solution-stacks.html) in the AWS CLI\)\. This operation returns all of the solution stacks that you can use to create an environment\.

**Note**  
You can use solution stacks for the latest platform configurations \(the current versions listed on this page\) to create an environment\.  
In addition, a platform configuration that you used to launch or update an environment remains available \(to the account in use, in the region used\) even after it's no longer current, as long as the environment is active, and up to 30 days after its termination\.

You can customize and configure the software that your application depends on in your Linux platform\. Learn more at [Customizing Software on Linux Servers](customize-containers-ec2.md)\. Detailed release notes are available for recent releases at [AWS Elastic Beanstalk Release Notes](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/relnotes.html)\. 

## Supported Platform Configurations<a name="concepts.platforms.list"></a>

All currently supported platform configurations are listed in [Elastic Beanstalk Supported Platforms](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html)\. For direct access to the configuration list of a specific platform, use one of the following links\.
+ [Packer Builder](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.packer)
+ [Single Container Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.docker)
+ [Multicontainer Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.mcdocker)
+ [Preconfigured Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.dockerpreconfig)
+ [Go](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.go)
+ [Java SE](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.javase)
+ [Java with Tomcat](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.java)
+ [\.NET on Windows Server with IIS](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net)
+ [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs)
+ [PHP](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP)
+ [Python](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.python)
+ [Ruby](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.ruby)