# Elastic Beanstalk supported platforms<a name="concepts.platforms"></a>

AWS Elastic Beanstalk provides a variety of platforms on which you can build your applications\. You design your web application to one of these platforms, and Elastic Beanstalk deploys your code to the platform version you selected to create an active application environment\.

Elastic Beanstalk provides platforms for programming languages \(Go, Java, Node\.js, PHP, Python, Ruby\), application servers \(Tomcat, Passenger, Puma\), and Docker containers\. Some platforms have multiple concurrently\-supported versions\.

Elastic Beanstalk provisions the resources needed to run your application, including one or more Amazon EC2 instances\. The software stack running on the Amazon EC2 instances depends on the specific platform version you've selected for your environment\.

You can use the solution stack name listed under the platform version name to launch an environment with the [EB CLI](eb-cli3.md), [Elastic Beanstalk API](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/), or [AWS CLI](https://aws.amazon.com/cli/)\. You can also retrieve solution stack names from the service with the [ListAvailableSolutionStacks](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html) API \([aws elasticbeanstalk list\-available\-solution\-stacks](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-available-solution-stacks.html) in the AWS CLI\)\. This operation returns all of the solution stacks that you can use to create an environment\.

**Note**  
Each platform has supported and retired platform versions\. You can always create an environment based on a supported platform version\. Retired platform versions are available only to existing customer environments for a period of 90 days from the published retirement date\. For a list of published platform version retirement dates, see [Retired platform branch schedule](platforms-support-policy.md#platforms-support-policy.depracation)\.  
When Elastic Beanstalk updates a platform, previous platform versions are still supported, but they lack the most up\-to\-date components and aren't recommended for use\. We recommend that you transition to the latest platform version\. You can still create an environment based on a previous platform version if you've used it in an environment in the last 30 days \(using the same account, in the same region\)\.

You can customize and configure the software that your application depends on in your platform\. Learn more at [Customizing software on Linux servers](customize-containers-ec2.md) and [Customizing software on Windows servers](customize-containers-windows-ec2.md)\. Detailed release notes are available for recent releases at [AWS Elastic Beanstalk Release Notes](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/)\. 

## Supported platform versions<a name="concepts.platforms.list"></a>

All current platform versions are listed in [Elastic Beanstalk Supported Platforms](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html) in the *AWS Elastic Beanstalk Platforms* guide\. Each platform\-specific section also points to the *platform history*, a list of previous platform versions\. For direct access to the version list of a specific platform, use one of the following links\.
+ [Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.docker)
+ [Multicontainer Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.mcdocker)
+ [Preconfigured Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.dockerpreconfig)
+ [Go](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.go)
+ [Java SE](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.javase)
+ [Tomcat](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.java)
+ [\.NET Core on Linux](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.dotnetlinux)
+ [\.NET on Windows Server](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net)
+ [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs)
+ [PHP](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP)
+ [Python](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.python)
+ [Ruby](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.ruby)