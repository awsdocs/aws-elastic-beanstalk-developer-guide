# Constructing a Launch Now URL<a name="launch-now-url"></a>

You can construct a custom uniform resource locator \(URL\) so that anyone can quickly deploy and run a predetermined web application in AWS Elastic Beanstalk\. This URL is called a Launch Now URL\. You might need a Launch Now URL, for example, to demonstrate a web application that is built to run on Elastic Beanstalk\. With a Launch Now URL, you can use parameters to add the required information to the Create Application wizard in advance\. When you do, anyone can use the URL link to launch an Elastic Beanstalk environment with your web application source in just a few steps\. This means users don't need to manually upload or specify the location of the application source bundle or provide any additional input to the wizard\.

A Launch Now URL gives Elastic Beanstalk the minimum information required to create an application: the application name, solution stack, instance type, and environment type\. Elastic Beanstalk uses default values for other configuration details that are not explicitly specified in your custom Launch Now URL\.

A Launch Now URL uses standard URL syntax\. For more information, see [RFC 3986 \- Uniform Resource Identifier \(URI\): Generic Syntax](http://tools.ietf.org/html/rfc3986)\.

## URL parameters<a name="launch-now-url.params"></a>

The URL must contain the following parameters, which are case\-sensitive:
+ **region** – Specify an AWS Region\. For a list of regions supported by Elastic Beanstalk, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html) in the *AWS General Reference*\.
+ **applicationName** – Specify the name of your application\. Elastic Beanstalk displays the application name in the Elastic Beanstalk console to distinguish it from other applications\. By default, the application name also forms the basis of the environment name and environment URL\.
+ **platform** – Specify the platform version to use for the environment\. Use one of the following methods, then URL\-encode your choice:
  + Specify a platform ARN without a version\. Elastic Beanstalk selects the latest platform version of the corresponding platform major version\. For example, to select the latest Python 3\.6 platform version, specify:

    `Python 3.6 running on 64bit Amazon Linux`
  + Specify the platform name\. Elastic Beanstalk selects the latest version of the platform's latest language runtime\. For example:

    `Python`

  For a description of all available platforms and their versions, see [Elastic Beanstalk supported platforms](concepts.platforms.md)\.

  You can use the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/) \(AWS CLI\) to get a list of available platform versions with their respective ARNs\. The `list-platform-versions` command lists detailed information about all available platform versions\. The `--filters` argument allows you to scope down the list\. For example, you can list all platform versions of a specific language\.

  The following example queries for all Python platform versions, and pipes the output through a series of commands\. The result is a list of platform version ARNs \(without the `/version` tail\), in human\-readable format, with no URL encoding\.

  ```
  $ aws elasticbeanstalk list-platform-versions --filters 'Type="PlatformName",Operator="contains",Values="Python"' | grep PlatformArn | awk -F '"' '{print $4}' | awk -F '/' '{print $2}'
  Preconfigured Docker - Python 3.4 running on 64bit Debian
  Preconfigured Docker - Python 3.4 running on 64bit Debian
  Python 2.6 running on 32bit Amazon Linux
  Python 2.6 running on 32bit Amazon Linux 2014.03
  ...
  Python 3.6 running on 64bit Amazon Linux
  ```

  The following example adds a Perl command to the last example, to URL\-encode the output\.

  ```
  $ aws elasticbeanstalk list-platform-versions --filters 'Type="PlatformName",Operator="contains",Values="Python"' | grep PlatformArn | awk -F '"' '{print $4}' | awk -F '/' '{print $2}' | perl -MURI::Escape -ne 'chomp;print uri_escape($_),"\n"'
  Preconfigured%20Docker%20-%20Python%203.4%20running%20on%2064bit%20Debian
  Preconfigured%20Docker%20-%20Python%203.4%20running%20on%2064bit%20Debian
  Python%202.6%20running%20on%2032bit%20Amazon%20Linux
  Python%202.6%20running%20on%2032bit%20Amazon%20Linux%202014.03
  ...
  Python%203.6%20running%20on%2064bit%20Amazon%20Linux
  ```

A Launch Now URL can optionally contain the following parameters\. If you don't include the optional parameters in your Launch Now URL, Elastic Beanstalk uses default values to create and run your application\. When you don't include the **sourceBundleUrl** parameter, Elastic Beanstalk uses the default sample application for the specified **platform**\.
+ **sourceBundleUrl** – Specify the location of your web application source bundle in URL format\. For example, if you uploaded your source bundle to an Amazon S3 bucket, you might specify the value of the **sourceBundleUrl** parameter as `https://mybucket.s3.amazonaws.com/myobject`\.
**Note**  
You can specify the value of the **sourceBundleUrl** parameter as an HTTP URL, but the user's web browser will convert characters as needed by applying HTML URL encoding\.
+ **environmentType** – Specify whether the environment is load balanced and scalable or just a single instance\. For more information, see [Environment types](using-features-managing-env-types.md)\. You can specify either `LoadBalancing` or `SingleInstance` as the parameter value\.
+ **tierName** – Specify whether the environment supports a web application that processes web requests or a web application that runs background jobs\. For more information, see [Elastic Beanstalk worker environments](using-features-managing-env-tiers.md)\. You can specify either `WebServer` or `Worker`,
+ **instanceType** – Specify a server with the characteristics \(including memory size and CPU power\) that are most appropriate to your application\. To see the instance types that are available in your Elastic Beanstalk region, see [InstanceType](command-options-general.md#option-instance-type) in [Configuration options](command-options.md)\. To see the detailed specifications for each Amazon EC2 instance type, see [Instance Types](https://aws.amazon.com/ec2/instance-types/#instance-details)\.
+ **withVpc** – Specify whether to create the environment in an Amazon VPC\. You can specify either `true` or `false`\. For more information about using Elastic Beanstalk with Amazon VPC, see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.
+ **withRds** – Specify whether to create an Amazon RDS database instance with this environment\. For more information, see [Using Elastic Beanstalk with Amazon RDS](AWSHowTo.RDS.md)\. You can specify either `true` or `false`\.
+ **rdsDBEngine** – Specify the database engine that you want to use for your Amazon EC2 instances in this environment\. You can specify `mysql`, `oracle-sel`, `sqlserver-ex`, `sqlserver-web`, or `sqlserver-se`\. The default value is `mysql`\.
+ **rdsDBAllocatedStorage** – Specify the allocated database storage size in gigabytes\. You can specify the following values:
  + **MySQL** – `5` to `1024`\. The default is `5`\.
  + **Oracle** – `10` to `1024`\. The default is `10`\.
  + **Microsoft SQL Server Express Edition** – `30`\.
  + **Microsoft SQL Server Web Edition** – `30`\.
  + **Microsoft SQL Server Standard Edition** – `200`\.
+ **rdsDBInstanceClass** – Specify the database instance type\. The default value is `db.t2.micro` \(`db.m1.large` for an environment not running in an Amazon VPC\)\. For a list of database instance classes supported by Amazon RDS, see [DB Instance Class](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.html) in the * Amazon Relational Database Service User Guide*\.
+ **rdsMultiAZDatabase** – Specify whether Elastic Beanstalk needs to create the database instance across multiple Availability Zones\. You can specify either `true` or `false`\. For more information about multiple Availability Zone deployments with Amazon RDS, go to [Regions and Availability Zones](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html) in the * Amazon Relational Database Service User Guide*\.
+ **rdsDBDeletionPolicy** – Specify whether to delete or snapshot the database instance on environment termination\. You can specify either `Delete` or `Snapshot`\.

## Example<a name="launch-now-url.example"></a>

The following is an example Launch Now URL\. After you construct your own, you can give it to your users\. For example, you might want to embed the URL on a webpage or in training materials\. When users create an application using the Launch Now URL, the Elastic Beanstalk Create an Application wizard requires no additional input\.

```
https://console.aws.amazon.com/elasticbeanstalk/home?region=us-west-2#/newApplication?applicationName=YourCompanySampleApp&platform=PHP%207.3%20running%20on%2064bit%20Amazon%20Linux&sourceBundleUrl=http://s3.amazonaws.com/mybucket/myobject&environmentType=SingleInstance&tierName=WebServer&instanceType=m1.small&withVpc=true&withRds=true&rdsDBEngine=postgres&rdsDBAllocatedStorage=6&rdsDBInstanceClass=db.m1.small&rdsMultiAZDatabase=true&rdsDBDeletionPolicy=Snapshot
```

When users choose a Launch Now URL, Elastic Beanstalk displays a page similar to the following\.

![\[Elastic Beanstalk management console page for a Launch Now URL\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-launch-now-page.png)

**To use the Launch Now URL**

1. Choose the Launch Now URL\.

1. When the Elastic Beanstalk console opens, on the **Create a web app** page, choose **Review and launch** to view the settings Elastic Beanstalk will use to create the application and launch the environment in which the application runs\.

1. On the **Configure** page, choose **Create app** to create the application\.