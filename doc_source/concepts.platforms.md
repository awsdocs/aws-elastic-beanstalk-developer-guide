# Elastic Beanstalk Supported Platforms<a name="concepts.platforms"></a>

AWS Elastic Beanstalk provides platforms for programming languages \(Java, PHP, Python, Ruby, Go\), web containers \(Tomcat, Passenger, Puma\) and Docker containers, with multiple configurations of each\.

Elastic Beanstalk provisions the resources needed to run your application, including one or more Amazon EC2 instances\. The software stack running on the Amazon EC2 instances depends on the configuration\. In a configuration name, the version number refers to the version of the platform configuration\.

You can use the solution stack name listed under the configuration name to launch an environment with the [EB CLI](eb-cli3.md), [Elastic Beanstalk API](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/), or [AWS CLI](https://aws.amazon.com/cli/)\. You can also retrieve solution stack names from the service with the [ListAvailableSolutionStacks](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html) API \([aws elasticbeanstalk list\-available\-solution\-stacks](http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-available-solution-stacks.html) in the AWS CLI\)\. This operation returns all of the solution stacks that you can use to create an environment\.

**Note**  
You can use solution stacks for the latest platform configurations \(the current versions listed on this page\) to create an environment\.  
In addition, a platform configuration that you used to launch or update an environment remains available \(to the account in use, in the region used\) even after it's no longer current, as long as the environment is active, and up to 30 days after its termination\.

All current Linux\-based platform configurations run on Amazon Linux 2017\.09 \(64\-bit\)\. You can customize and configure the software that your application depends on in your Linux platform\. Learn more at [Customizing Software on Linux Servers](customize-containers-ec2.md)\. Detailed release notes are available for recent releases at [aws\.amazon\.com/releasenotes](https://aws.amazon.com/releasenotes/AWS-Elastic-Beanstalk)\. 

**Topics**
+ [Packer Builder](#concepts.platforms.packer)
+ [Single Container Docker](#concepts.platforms.docker)
+ [Multicontainer Docker](#concepts.platforms.mcdocker)
+ [Preconfigured Docker](#concepts.platforms.dockerpreconfig)
+ [Go](#concepts.platforms.go)
+ [Java SE](#concepts.platforms.javase)
+ [Java with Tomcat](#concepts.platforms.java)
+ [\.NET on Windows Server with IIS](#concepts.platforms.net)
+ [Node\.js](#concepts.platforms.nodejs)
+ [PHP](#concepts.platforms.PHP)
+ [Python](#concepts.platforms.python)
+ [Ruby](#concepts.platforms.ruby)

## Packer Builder<a name="concepts.platforms.packer"></a>

Packer is an open\-source tool for creating machine images for many platforms, including AMIs for use with Amazon EC2\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Packer Version  | 
| --- | --- | --- | 
|   **Elastic Beanstalk Packer Builder version 2\.5\.1**   * 64bit Amazon Linux 2018\.03 v2\.5\.0 running Packer 1\.0\.3 *   |  2018\.03\.0  |  1\.0\.3  | 

For information about previous configurations, see [Packer Platform History](platform-history-packer.md)\.

## Single Container Docker<a name="concepts.platforms.docker"></a>

Docker is a container platform that allows you to define your own software stack and store it in an image that can be downloaded from a remote repository\. Use the Single Container Docker platform if you only need to run a single Docker container on each instance in your environment\. The single container configuration includes an nginx proxy server\.

See [Deploying Elastic Beanstalk Applications from Docker Containers](create_deploy_docker.md) for more information about the Docker platform\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Docker Version  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Single Container Docker 18\.03 version 2\.11\.0**   * 64bit Amazon Linux 2018\.03 v2\.11\.0 running Docker 18\.03\.1\-ce *   |  2018\.03\.0  |  18\.03\.1\-ce  |  nginx 1\.12\.1  | 

For information about previous configurations, see [Single Container Docker Platform History](platform-history-docker-single.md)\.

## Multicontainer Docker<a name="concepts.platforms.mcdocker"></a>

Docker is a container platform that allows you to define your own software stack and store it in an image that can be downloaded from a remote repository\. Use the Multicontainer Docker platform if you need to run multiple containers on each instance\. The Multicontainer Docker platform does not include a proxy server\.

**Note**  
Elastic Beanstalk uses Amazon Elastic Container Service \(Amazon ECS\) to coordinate container deployments to multicontainer Docker environments\. Some regions don't offer Amazon ECS\. Multicontainer Docker isn't supported in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

See [Deploying Elastic Beanstalk Applications from Docker Containers](create_deploy_docker.md) for more information about the Docker platform\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Docker Version  |  ECS Agent  | 
| --- | --- | --- | --- | 
|   **Multicontainer Docker 18\.03 version 2\.11\.0**   * 64bit Amazon Linux 2018\.03 v2\.11\.0 running Multi\-container Docker 18\.03\.1\-ce \(Generic\) *   |  2018\.03\.0  |  18\.03\.1\-ce  |  1\.18\.0  | 

For information about previous configurations, see [Multicontainer Docker Platform History](platform-history-docker-multi.md)\.

## Preconfigured Docker<a name="concepts.platforms.dockerpreconfig"></a>

Preconfigured Docker platform configurations use Docker, but do not let you provide your own Docker images\. The preconfigured containers provide application frameworks that are not available on other platforms, such as Glassfish\. They also provide support for Go applications prior to the release of the full\-fledged Go platform\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Platform  |  Container OS  |  Language  |  Proxy Server  |  Application Server  |  Docker Image  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|   **Glassfish 5\.0 \(Docker\) version 2\.11\.0**   * 64bit Amazon Linux v2\.11\.0 running GlassFish 5\.0 Java 8 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Amazon Linux 2018\.03  |  Java 8  |  nginx 1\.12\.1  |  Glassfish 5\.0  |  amazon/aws\-eb\-glassfish:5\.0\-al  | 
|   **Glassfish 4\.1 \(Docker\) version 2\.11\.0**   * 64bit Amazon Linux v2\.11\.0 running GlassFish 4\.1 Java 8 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Amazon Linux 2018\.03  |  Java 8  |  nginx 1\.12\.1  |  Glassfish 4\.1  |  amazon/aws\-eb\-glassfish:4\.1\.2\-al  | 
|   **Glassfish 4\.1 \(Docker\) version 2\.11\.0**   * 64bit Debian jessie v2\.11\.0 running GlassFish 4\.1 Java 8 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Debian Jessie  |  Java 8  |  nginx 1\.12\.1  |  Glassfish 4\.1  |  amazon/aws\-eb\-glassfish:4\.1\-jdk8\-onbuild\-3\.5\.1  | 
|   **Glassfish 4\.0 \(Docker\) version 2\.11\.0**   * 64bit Debian jessie v2\.10\.0 running GlassFish 4\.0 Java 7 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Debian Jessie  |  Java 7  |  nginx 1\.12\.1  |  Glassfish 4\.0  |  amazon/aws\-eb\-glassfish:4\.0\-jdk7\-onbuild\-3\.5\.1  | 
|   **Go 1\.4 \(Docker\) version 2\.11\.0**   * 64bit Debian jessie v2\.11\.0 running Go 1\.4 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Debian Jessie  |  Go 1\.4\.2  |  nginx 1\.12\.1  |  none  |  golang:1\.4\.2\-onbuild  | 
|   **Go 1\.3 \(Docker\) version 2\.11\.0**   * 64bit Debian jessie v2\.11\.0 running Go 1\.3 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Debian Jessie  |  Go 1\.3\.3  |  nginx 1\.12\.1  |  none  |  golang:1\.3\.3\-onbuild  | 
|   **Python 3\.4 with uWSGI 2 \(Docker\) version 2\.11\.0**   * 64bit Debian jessie v2\.11\.0 running Python 3\.4 \(Preconfigured \- Docker\) *   |  2018\.03\.0  |  Docker 18\.03\.1\-ce  |  Debian Jessie  |  Python 3\.4  |  nginx 1\.12\.1  |  uWSGI 2\.0\.8  |  amazon/aws\-eb\-python:3\.4\.2\-onbuild\-3\.5\.1  | 

For information about previous configurations, see [Preconfigured Docker Platform History](platform-history-preconfigureddocker.md)\.

## Go<a name="concepts.platforms.go"></a>

Elastic Beanstalk supports the following Go configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Language  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Go 1\.10 version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Go 1\.10 *   |  2018\.03\.0  |  Go 1\.10  |  nginx 1\.12\.1  | 

For information about previous configurations, see [Go Platform History](platform-history-go.md)\.

## Java SE<a name="concepts.platforms.javase"></a>

Elastic Beanstalk supports the following Java SE configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Language  |  Tools  |  AWS X‑Ray  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | 
|   **Java 8 version 2\.7\.2**   * 64bit Amazon Linux 2018\.03 v2\.7\.2 running Java 8 *   |  2018\.03\.0  |  Java 1\.8\.0\_171  |  Ant 1\.9\.6, Gradle 2\.7, Maven 3\.3\.3  |  2\.0\.0  |  nginx 1\.12\.1  | 
|   **Java 7 version 2\.7\.2**   * 64bit Amazon Linux 2018\.03 v2\.7\.2 running Java 7 *   |  2018\.03\.0  |  Java 1\.7\.0\_181  |  Ant 1\.9\.6, Gradle 2\.7, Maven 3\.3\.3  |  2\.0\.0  |  nginx 1\.12\.1  | 

For information about previous configurations, see [Java SE Platform History](platform-history-javase.md)\.

## Java with Tomcat<a name="concepts.platforms.java"></a>

Elastic Beanstalk supports the following Tomcat configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Language  |  AWS X‑Ray  |  Application Server  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | 
|   **Java 8 with Tomcat 8\.5 version 3\.0\.1**   * 64bit Amazon Linux 2018\.03 v3\.0\.1 running Tomcat 8\.5 Java 8 *   |  2018\.03\.0  |  Java 1\.8\.0\_171  |  2\.0\.0  |  Tomcat 8\.5\.29  |  Apache 2\.4\.33 \(default\), Apache 2\.2\.34, Nginx 1\.12\.1  | 
|   **Java 8 with Tomcat 8 version 3\.0\.1**   * 64bit Amazon Linux 2018\.03 v3\.0\.1 running Tomcat 8 Java 8 *   |  2018\.03\.0  |  Java 1\.8\.0\_171  |  2\.0\.0  |  Tomcat 8\.0\.50  |  Apache 2\.4\.33 \(default\), Apache 2\.2\.34, Nginx 1\.12\.1  | 
|   **Java 7 with Tomcat 7 version 3\.0\.1**   * 64bit Amazon Linux 2018\.03 v3\.0\.1 running Tomcat 7 Java 7 *   |  2018\.03\.0  |  Java 1\.7\.0\_181  |  2\.0\.0  |  Tomcat 7\.0\.85  |  Apache 2\.4\.33 \(default\), Apache 2\.2\.34, Nginx 1\.12\.1  | 
|   **Java 6 with Tomcat 7 version 3\.0\.1**   * 64bit Amazon Linux 2018\.03 v3\.0\.1 running Tomcat 7 Java 6 *   |  2018\.03\.0  |  Java 1\.6\.0\_41  |  2\.0\.0  |  Tomcat 7\.0\.85  |  Apache 2\.4\.33 \(default\), Apache 2\.2\.34, Nginx 1\.12\.1  | 

For information about previous configurations, see [Tomcat Platform History](platform-history-java.md)\.

## \.NET on Windows Server with IIS<a name="concepts.platforms.net"></a>

You can get started in minutes using the [AWS Toolkit for Visual Studio](http://aws.amazon.com/visualstudio/)\. The toolkit includes the AWS libraries, project templates, code samples, and documentation\. The AWS SDK for \.NET supports the development of applications using \.NET Framework 2\.0 or later\. 

**Note**  
This platform doesn't support worker environments, enhanced health reporting, managed updates, bundle logs, and immutable updates\.

To learn how to get started deploying a \.NET application using the AWS Toolkit for Visual Studio, see [Creating and Deploying Elastic Beanstalk Applications in \.NET Using AWS Toolkit for Visual Studio](create_deploy_NET.md)\.

For information about the latest Microsoft security updates, see [Security TechCenter](https://portal.msrc.microsoft.com/en-us/) and [Security Advisories and Bulletins](https://technet.microsoft.com/en-us/library/security/)\.

For information about previous \.NET configurations for Elastic Beanstalk, see [\.NET on Windows Server with IIS Platform History](platform-history-dotnet.md)\.

**Note**  
To use the C5 instance type family, choose Windows Server 2012 R2 or newer\.

Elastic Beanstalk supports the following \.NET configurations\.

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |   * 64bit Windows Server 2016 v1\.2\.0 running IIS 10\.0 *   |  \.NET Core 2\.1, supports 2\.1\.x, 2\.0\.x, 1\.1\.x, 1\.0\.x \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 10\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |   * 64bit Windows Server Core 2016 v1\.2\.0 running IIS 10\.0 *   |  \.NET Core 2\.1, supports 2\.1\.x, 2\.0\.x, 1\.1\.x, 1\.0\.x \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 10\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |   * 64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5 *   |  \.NET Core 2\.1, supports 2\.1\.x, 2\.0\.x, 1\.1\.x, 1\.0\.x \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |   * 64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5 *   |  \.NET Core 2\.1, supports 2\.1\.x, 2\.0\.x, 1\.1\.x, 1\.0\.x \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |   * 64bit Windows Server 2012 v1\.2\.0 running IIS 8 *   |  \.NET Core 2\.1, supports 2\.1\.x, 2\.0\.x, 1\.1\.x, 1\.0\.x \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |   * 64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5 *   |  \.NET Core 2\.1, supports 2\.1\.x, 2\.0\.x, 1\.1\.x, 1\.0\.x \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |   * 64bit Windows Server 2012 R2 running IIS 8\.5 *   |  \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |   * 64bit Windows Server Core 2012 R2 running IIS 8\.5 *   |  \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   |   * 64bit Windows Server 2012 running IIS 8 *   |  \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |   * 64bit Windows Server 2008 R2 running IIS 7\.5 *   |  \.NET Framework 4\.7\.2, supports 4\.x, 2\.0, 1\.x  |  IIS 7\.5  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |  2018\.07\.11  |  3\.3\.311\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |  2018\.07\.11  |  3\.3\.311\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |  2018\.07\.11  |  3\.3\.311\.0  |  4\.9\.2756  |  2\.2\.800\.0  |  3\.6  |  1\.0\.0  | 

## Node\.js<a name="concepts.platforms.nodejs"></a>

The Node\.js platform includes a few Node\.js versions in a single configuration\. The following table lists them\. The default version applies when the NodeVersion option in the `aws:elasticbeanstalk:container:nodejs` namespace isn't set\. See [Node\.js Platform Options](command-options-specific.md#command-options-nodejs) for details\.

Each Node\.js version includes a respective version of npm \(the Node\.js package manager\)\. The table lists npm versions in parentheses\.

Elastic Beanstalk supports the following Node\.js configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Node\.js version \(npm version\)  |  Proxy Server  |  Git  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | 
|   **Node\.js version 4\.5\.1**   * 64bit Amazon Linux 2018\.03 v4\.5\.1 running Node\.js *   |  2018\.03\.0  |  8\.11\.3 \(5\.6\.0\), 8\.11\.1\(5\.6\.0\), 7\.10\.1 \(4\.2\.0\), 6\.14\.3 \(3\.10\.10\), 6\.14\.1\(3\.10\.10\), 5\.12\.0 \(3\.8\.6\), 4\.9\.1\(2\.15\.11\), 4\.8\.7 \(2\.15\.11\)  Default platform: 6\.14\.3  |  nginx 1\.12\.1, Apache 2\.4\.27  |  2\.14\.4  |  2\.0\.0  | 

For information about previous configurations, see [Node\.js Platform History](platform-history-nodejs.md)\.

**Note**  
When support for the version of Node\.js that you are using is removed from the platform configuration, you must change or remove the version setting prior to doing a [platform upgrade](using-features.platform.upgrade.md)\. This may occur when a security vulnerability is identified for one or more versions of Node\.js  
When this occurs, attempting to upgrade to a new version of the platform that does not support the configured [NodeVersion](command-options-specific.md#command-options-nodejs) will fail\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a version that is supported by both the old configuration version and the new one, or [remove the option setting](environment-configuration-methods-after.md), and then perform the platform upgrade\.

## PHP<a name="concepts.platforms.PHP"></a>

Elastic Beanstalk supports the following PHP configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Language  |  Composer  |  Proxy Server  | 
| --- | --- | --- | --- | --- | 
|   **PHP 7\.1 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running PHP 7\.1 *   |  2018\.03\.0  |  PHP 7\.1\.17  |  1\.4\.2  |  Apache 2\.4\.27  | 
|   **PHP 7\.0 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running PHP 7\.0 *   |  2018\.03\.0  |  PHP 7\.0\.30  |  1\.4\.2  |  Apache 2\.4\.27  | 
|   **PHP 5\.6 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running PHP 5\.6 *   |  2018\.03\.0  |  PHP 5\.6\.36  |  1\.4\.2  |  Apache 2\.4\.27  | 
|   **PHP 5\.5 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running PHP 5\.5 *   |  2018\.03\.0  |  PHP 5\.5\.38  |  1\.4\.2  |  Apache 2\.4\.27  | 
|   **PHP 5\.4 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running PHP 5\.4 *   |  2018\.03\.0  |  PHP 5\.4\.45  |  1\.4\.2  |  Apache 2\.4\.27  | 

For information about previous configurations, see [PHP Platform History](platform-history-php.md)\.

## Python<a name="concepts.platforms.python"></a>

Elastic Beanstalk supports the following Python configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Language  |  Package Manager  |  Packager  |  meld3  |  AWS X‑Ray  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|   **Python 3\.6 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running Python 3\.6 *   |  2018\.03\.0  |  Python 3\.6\.5  |  pip 9\.0\.3  |  setuptools 28\.8\.0  |  meld3 1\.0\.2  |  2\.0\.0  |  Apache 2\.4\.27 with mod\_wsgi 3\.5  | 
|   **Python 3\.4 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running Python 3\.4 *   |  2018\.03\.0  |  Python 3\.4\.8  |  pip 9\.0\.3  |  setuptools 28\.8\.0  |  meld3 1\.0\.2  |  2\.0\.0  |  Apache 2\.4\.27 with mod\_wsgi 3\.5  | 
|   **Python 2\.7 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running Python 2\.7 *   |  2018\.03\.0  |  Python 2\.7\.14  |  pip 9\.0\.3  |  setuptools 28\.8\.0  |  meld3 1\.0\.2  |  2\.0\.0  |  Apache 2\.4\.27 with mod\_wsgi 3\.5  | 
|   **Python 2\.6 version 2\.7\.1**   * 64bit Amazon Linux 2018\.03 v2\.7\.1 running Python 2\.6 *   |  2018\.03\.0  |  Python 2\.6\.9  |  pip 9\.0\.3  |  setuptools 28\.8\.0  |  meld3 1\.0\.2  |  2\.0\.0  |  Apache 2\.4\.27 with mod\_wsgi 3\.5  | 

For information about previous configurations, see [Python Platform History](platform-history-python.md)\.

## Ruby<a name="concepts.platforms.ruby"></a>

Elastic Beanstalk supports the following Ruby configurations\.


****  

|  Configuration and *Solution Stack Name*   |  AMI  |  Language  |  Package Manager  |  Application Server  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | 
|   **Ruby 2\.5 with Puma version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.5 \(Puma\) *   |  2018\.03\.0  |  Ruby 2\.5\.1\-p57  |  RubyGems 2\.7\.6  |  Puma 2\.16\.0  |  nginx 1\.12\.1  | 
|   **Ruby 2\.5 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.5 \(Passenger Standalone\) *   |  2018\.03\.0  |  Ruby 2\.5\.1\-p57  |  RubyGems 2\.7\.6  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 
|   **Ruby 2\.4 with Puma version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.4 \(Puma\) *   |  2018\.03\.0  |  Ruby 2\.4\.4\-p296  |  RubyGems 2\.7\.6  |  Puma 2\.16\.0  |  nginx 1\.12\.1  | 
|   **Ruby 2\.4 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.4 \(Passenger Standalone\) *   |  2018\.03\.0  |  Ruby 2\.4\.4\-p296  |  RubyGems 2\.7\.6  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 
|   **Ruby 2\.3 with Puma version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.3 \(Puma\) *   |  2018\.03\.0  |  Ruby 2\.3\.7\-p456  |  RubyGems 2\.7\.6  |  Puma 2\.16\.0  |  nginx 1\.12\.1  | 
|   **Ruby 2\.3 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.3 \(Passenger Standalone\) *   |  2018\.03\.0  |  Ruby 2\.3\.7\-p456  |  RubyGems 2\.7\.6  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 
|   **Ruby 2\.2 with Puma version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.2 \(Puma\) *   |  2018\.03\.0  |  Ruby 2\.2\.10\-p489  |  RubyGems 2\.7\.6  |  Puma 2\.16\.0  |  nginx 1\.12\.1  | 
|   **Ruby 2\.2 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.2 \(Passenger Standalone\) *   |  2018\.03\.0  |  Ruby 2\.2\.10\-p489  |  RubyGems 2\.7\.6  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 
|   **Ruby 2\.1 with Puma version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.1 \(Puma\) *   |  2018\.03\.0  |  Ruby 2\.1\.10\-p492  |  RubyGems 2\.6\.13  |  Puma 2\.16\.0  |  nginx 1\.12\.1  | 
|   **Ruby 2\.1 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.1 \(Passenger Standalone\) *   |  2018\.03\.0  |  Ruby 2\.1\.10\-p492  |  RubyGems 2\.6\.13  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 
|   **Ruby 2\.0 with Puma version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.0 \(Puma\) *   |  2018\.03\.0  |  Ruby 2\.0\.0\-p648  |  RubyGems 2\.6\.13  |  Puma 2\.16\.0  |  nginx 1\.12\.1  | 
|   **Ruby 2\.0 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 2\.0 \(Passenger Standalone\) *   |  2018\.03\.0  |  Ruby 2\.0\.0\-p648  |  RubyGems 2\.6\.13  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 
|   **Ruby 1\.9 with Passenger version 2\.8\.1**   * 64bit Amazon Linux 2018\.03 v2\.8\.1 running Ruby 1\.9\.3 *   |  2018\.03\.0  |  Ruby 1\.9\.3\-p551  |  RubyGems 2\.6\.13  |  Passenger 4\.0\.60  |  nginx 1\.12\.1  | 

For information about previous configurations, see [Ruby Platform History](platform-history-ruby.md)\.