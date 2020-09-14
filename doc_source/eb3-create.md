# eb create<a name="eb3-create"></a>

## Description<a name="eb3-createdescription"></a>

Creates a new environment and deploys an application version to it\.

**Note**  
To use eb create on a \.NET application, you must create a deployment package as described in [Creating a source bundle for a \.NET application](applications-sourcebundle.md#using-features.deployment.source.dotnet), then set up the CLI configuration to deploy the package as an artifact as described in [Deploying an artifact instead of the project folder](eb-cli3-configuration.md#eb-cli3-artifact)\.
Creating environments with the EB CLI requires a [service role](concepts-roles-service.md)\. You can create a service role by creating an environment in the Elastic Beanstalk console\. If you don't have a service role, the EB CLI attempts to create one when you run `eb create`\.

You can deploy the application version from a few sources:
+ By default: from the application source code in the local project directory\.
+ Using the `--version` option: from an application version that already exists in your application\.
+ When your project directory doesn't have application code, or when using the `--sample` option: from a sample application, specific to your environment's platform\.

## Syntax<a name="eb3-createsyntax"></a>

eb create

eb create *environment\-name*

An environment name must be between 4 and 40 characters in length, and can only contain letters, numbers, and hyphens\. An environment name can't begin or end with a hyphen\.

If you include an environment name in the command, the EB CLI doesn't prompt you to make any selections or create a service role\.

If you run the command without an environment name argument, it runs in an interactive flow, and prompts you to enter or select values for some settings\. In this interactive flow, in case you are deploying a sample application, the EB CLI also asks you if you want to download this sample application to your local project directory\. This enables you to use the EB CLI with the new environment later to run operations that require the application's code, like [eb deploy](eb3-deploy.md)\.

Some interactive flow prompts are displayed only under certain conditions\. For example, if you choose to use an Application Load Balancer, and your account has at least one sharable Application Load Balancer, Elastic Beanstalk displays a prompt that asks if you want to use a shared load balancer\. This prompt isn't displayed if no sharable Application Load Balancer exists in your account\.

## Options<a name="eb3-createoptions"></a>

None of these options are required\. If you run eb create without any options, the EB CLI prompts you to enter or select a value for each setting\.


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-d` or `--branch_default`  |  Set the environment as the default environment for the current repository\.  | 
|  `--cfg` *config\-name*  |  [Use platform settings from a saved configuration](environment-configuration-methods-during.md#configuration-options-during-ebcli-savedconfig) in `.elasticbeanstalk/saved_configs/` or your Amazon S3 bucket\. Specify the name of the file only, without the `.cfg.yml` extension\.  | 
|  `-c` *subdomain\-name* or `--cname` *subdomain\-name*  |  The subdomain name to prefix the CNAME DNS entry that routes to your website\. Type: String Default: The environment name  | 
|  `-db` or `--database`  |  Attaches a database to the environment\. If you run eb create with the `--database` option, but without the `--database.username` and `--database.password` options, EB CLI prompts you for the database master user name and password\.  | 
|  `-db.engine` *engine* or `--database.engine` *engine*  |  The database engine type\. If you run eb create with this option, then EB CLI launches the environment with a database attached even if you didn't run the command with the `--database` option\. Type: String Valid values: `mysql`, `oracle-se1`, `postgres`, `sqlserver-ex`, `sqlserver-web`, `sqlserver-se`  | 
|  `-db.i` *instance\_type* or `--database.instance` *instance\_type*  |  The type of Amazon EC2 instance to use for the database\. If you run eb create with this option, then EB CLI launches the environment with a database attached even if you didn't run the command with the `--database` option\. Type: String Valid values: See [Option Values](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html)\.  | 
|  `-db.pass` *password* or `--database.password` *password*  |  The password for the database\. If you run eb create with this option, then EB CLI launches the environment with a database attached even if you didn't run the command with the `--database` option\.  | 
|  `-db.size` *number\_of\_gigabytes* or `--database.size` *number\_of\_gigabytes*  |  The number of gigabytes \(GB\) to allocate for database storage\. If you run eb create with this option, then EB CLI launches the environment with a database attached even if you didn't run the command with the `--database` option\. Type: Number Valid values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html)  | 
|  `-db.user` *username* or `--database.username` *username*  |  The user name for the database\. If you run eb create with this option, then EB CLI launches the environment with a database attached even if you didn't run the command with the `--database` option\. If you run eb create with the `--database` option, but without the `--database.username` and `--database.password` options, then EB CLI prompts you for the master database user name and password\.  | 
|  `-db.version` *version* or `--database.version` *version*  |  Allows you to specify the database engine version\. If this flag is present, the environment will launch with a database with the specified version number, even if the `--database` flag is not present\.  | 
|  `--elb-type` *type*  |  The [load balancer type](using-features.managing.elb.md)\. Type: String Valid values: `classic`, `application`, `network` Default: `application`  | 
|  `-es` or `--enable-spot`  |  Enable Spot Instance requests for your environment\. For more details, see [Auto Scaling group](using-features.managing.as.md)\. Related options: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html)  | 
| \-\-env\-group\-suffix groupname | Group name to append to the environment name\. Only for use with [Compose Environments](ebcli-compose.md)\. | 
|  `--envvars`  |  [Environment properties](environments-cfg-softwaresettings.md) in a comma\-separated list with the format *name*=*value*\. See [Configuring environment properties](environments-cfg-softwaresettings.md#environments-cfg-softwaresettings-console) for limits\.  | 
|  `-ip` *profile\_name* or `--instance_profile` *profile\_name*  |  The instance profile with the IAM role with the temporary security credentials that your application needs to access AWS resources\.  | 
|  `-it` or `-﻿-﻿instance-types type1[,type2 ...]`  |  A comma\-separated list of Amazon EC2 instance types you want your environment to use\. If you don't specify this option, Elastic Beanstalk provides default instance types\. For more details, see [Amazon EC2 instances](using-features.managing.ec2.md) and [Auto Scaling group](using-features.managing.as.md)\.  | 
|  `-i` or `--instance_type`  |  The Amazon EC2 instance type you want your environment to use\. If you don't specify this option, Elastic Beanstalk provides a default instance type\. For more details, see [Amazon EC2 instances](using-features.managing.ec2.md)\. The `--instance_type` option is obsolete\. It's replaced by the newer and more powerful `--instance-types` option\. The new option allows you to specify a list of one or more instance types for your environment\. The first value on that list is equivalent to the value of the `--instance_type` option\. The recommended way to specify instance types is by using the new option\.  | 
|  `-k` *key\_name* or `--keyname` *key\_name*  |  The name of the Amazon EC2 key pair to use with the Secure Shell \(SSH\) client to securely log in to the Amazon EC2 instances running your Elastic Beanstalk application\. If you include this option with the eb create command, the value you provide overwrites any key name that you might have specified with eb init\. Valid values: An existing key name that is registered with Amazon EC2  | 
|  `-im` *number\-of\-instances* or `--min-instances` *number\-of\-instances*  |  The minimal number of Amazon EC2 instances you require your environment to have\. Type: Number \(integer\) Default: `1` Valid values: `1` to `10000`  | 
|  `-ix` *number\-of\-instances* or `--max-instances` *number\-of\-instances*  |  The maximal number of Amazon EC2 instances you allow your environment to have\. Type: Number \(integer\) Default: `4` Valid values: `1` to `10000`  | 
|  `--modules` *component\-a component\-b*  | List of component environments to create\. Only for use with [Compose Environments](ebcli-compose.md)\. | 
|  `-sb` or `--on-demand-base-capacity`  |  The minimal number of On\-Demand Instances that your Auto Scaling group provisions before considering Spot Instances as your environment scales up\. This option can only be specified with the `--enable-spot` option\. For more details, see [Auto Scaling group](using-features.managing.as.md)\. Type: Number \(integer\) Default: `0` Valid values: `0` to `--max-instances` \(when absent: `MaxSize` option in [`aws:autoscaling:asg`](command-options-general.md#command-options-general-autoscalingasg) namespace\)  | 
|  `-sp` or `--on-demand-above-base-capacity`  |  The percentage of On\-Demand Instances as part of additional capacity that your Auto Scaling group provisions beyond the number of instances specified by the `--on-demand-base-capacity` option\. This option can only be specified with the `--enable-spot` option\. For more details, see [Auto Scaling group](using-features.managing.as.md)\. Type: Number \(integer\) Default: `0` for a single\-instance environment; `70` for a load\-balanced environment Valid values: `0` to `100`  | 
|  `-p` *platform\-version* or `--platform` *platform\-version*  |  The [platform version](concepts.platforms.md) to use\. You can specify a platform, a platform and version, a platform branch, a solution stack name, or a solution stack ARN\. For example: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html) Use [`eb platform list`](eb3-platform.md) to get a list of available configurations\. If you specify the `--platform` option, it overrides the value that was provided during `eb init`\.  | 
|  `-pr` or `--process`  |  Preprocess and validate the environment manifest and configuration files in the source bundle\. Validating configuration files can identify issues prior to deploying the application version to an environment\.  | 
|  `-r` *region* or `--region` *region*  |  The AWS Region in which you want to deploy the application\. For the list of values you can specify for this option, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html) in the *AWS General Reference*\.  | 
|  `--sample`  |  Deploy the sample application to the new environment instead of the code in your repository\.  | 
|  `--scale` *number\-of\-instances*  |  Launch with the specified number of instances  | 
| \-\-service\-role servicerole | Assign a non\-default service role to the environment\.  Do not enter an ARN, just the role name\. Elastic Beanstalk prefixes the role name with the correct values to create the resulting ARN internally\.  | 
|  `-ls` *load\-balancer* or `--shared-lb` *load\-balancer*  |  Configure the environment to use a shared load balancer\. Provide the name or ARN of a sharable load balancer in your account—an Application Load Balancer that you explicitly created, not one created by another Elastic Beanstalk environment\. For details, see [Shared Application Load Balancer](environments-cfg-alb-shared.md)\. Parameter examples: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html) You can specify this option only with `--elb-type application`\. If you specify that option and don't specify `--shared-lb`, Elastic Beanstalk creates a dedicated load balancer for the environment\.  | 
|  `-lp` *port* or `--shared-lb-port` *port*  |  The default listener port of the shared load balancer for this environment\. Elastic Beanstalk adds a listener rule that routes all traffic from this listener to the default environment process\. For details, see [Shared Application Load Balancer](environments-cfg-alb-shared.md)\. Type: Number \(integer\) Default: `80` Valid values: Any integer that represents a listener port of the shared load balancer\.  | 
|  `--single`  |  Create the environment with a single Amazon EC2 instance and without a load balancer\.  A single\-instance environment isn't production ready\. If the instance becomes unstable during deployment, or Elastic Beanstalk terminates and restarts the instance during a configuration update, your application can be unavailable for a period of time\. Use single\-instance environments for development, testing, or staging\. Use load\-balanced environments for production\.   | 
|  `-sm` or `--spot-max-price`  |  The maximum price per unit hour, in US$, that you're willing to pay for a Spot Instance\. This option can only be specified with the `--enable-spot` option\. For more details, see [Auto Scaling group](using-features.managing.as.md)\. Type: Number \(float\) Default: The On\-Demand price Valid values: `0.001` to `20.0`  | 
|  `-﻿-﻿tags key1=value1[,key2=value2 ...]`  |  Tag the resources in your environment\. Tags are specified as a comma\-separated list of `key=value` pairs\. For more details, see [Tagging environments](using-features.tagging.md)\.  | 
|  `-t worker` or `--tier worker`  | Create a worker environment\. Omit this option to create a web server environment\. | 
|  `--timeout` *minutes*  |  Set number of minutes before the command times out\.  | 
|  `--version` *version\_label*  |  Specifies the application version that you want deployed to the environment instead of the application source code in the local project directory\. Type: String Valid values: An existing application version label  | 
|  `--vpc`  |  Configure a VPC for your environment\. When you include this option, the EB CLI prompts you to enter all required settings prior to launching the environment\.  | 
|  `--vpc.dbsubnets subnet1,subnet2`  |  Specifies subnets for database instances in a VPC\. Required when `--vpc.id` is specified\.  | 
|  `--vpc.ec2subnets subnet1,subnet2`  |  Specifies subnets for Amazon EC2 instances in a VPC\. Required when `--vpc.id` is specified\.  | 
|  `--vpc.elbpublic`  |  Launches your Elastic Load Balancing load balancer in a public subnet in your VPC\. You can't specify this option with the `--tier worker` or `--single` options\.  | 
|  `--vpc.elbsubnets subnet1,subnet2`  |  Specifies subnets for the Elastic Load Balancing load balancer in a VPC\. You can't specify this option with the `--tier worker` or `--single` options\.  | 
|  `--vpc.id ID`  |  Launches your environment in the specified VPC\.  | 
|  `--vpc.publicip`  |  Launches your Amazon EC2 instances in a public subnet in your VPC\. You can't specify this option with the `--tier worker` option\.  | 
|  `--vpc.securitygroups securitygroup1,securitygroup2`  |  Specifies security group IDs\. Required when `--vpc.id` is specified\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-createoutput"></a>

If successful, the command prompts you with questions and then returns the status of the create operation\. If there were problems during the launch, you can use the [eb events](eb3-events.md) operation to get more details\.

If you enabled CodeBuild support in your application, eb create displays information from CodeBuild as your code is built\. For information about CodeBuild support in Elastic Beanstalk, see [Using the EB CLI with AWS CodeBuild](eb-cli-codebuild.md)\.

## Examples<a name="eb3-createexample1"></a>

The following example creates an environment in interactive mode\.

```
$ eb create
Enter Environment Name
(default is tmp-dev): ENTER
Enter DNS CNAME prefix
(default is tmp-dev): ENTER
Select a load balancer type
1) classic
2) application
3) network
(default is 2): ENTER
Environment details for: tmp-dev
  Application name: tmp
  Region: us-east-2
  Deployed Version: app-141029_145448
  Environment ID: e-um3yfrzq22
  Platform: 64bit Amazon Linux 2014.09 v1.0.9 running PHP 5.5
  Tier: WebServer-Standard-1.0
  CNAME: tmp-dev.elasticbeanstalk.com
  Updated: 2014-10-29 21:54:51.063000+00:00
Printing Status:
...
```

The following example also creates an environment in interactive mode\. In this example, your project directory doesn't have application code\. The command deploys a sample application and downloads it to your local project directory\.

```
$ eb create
Enter Environment Name
(default is tmp-dev): ENTER
Enter DNS CNAME prefix
(default is tmp-dev): ENTER
Select a load balancer type
1) classic
2) application
3) network
(default is 2): ENTER
NOTE: The current directory does not contain any source code. Elastic Beanstalk is launching the sample application instead.
Do you want to download the sample application into the current directory?
(Y/n): ENTER
INFO: Downloading sample application to the current directory.
INFO: Download complete.
Environment details for: tmp-dev
  Application name: tmp
  Region: us-east-2
  Deployed Version: Sample Application
  Environment ID: e-um3yfrzq22
  Platform: 64bit Amazon Linux 2014.09 v1.0.9 running PHP 5.5
  Tier: WebServer-Standard-1.0
  CNAME: tmp-dev.elasticbeanstalk.com
  Updated: 2017-11-08 21:54:51.063000+00:00
Printing Status:
...
```

The following command creates an environment without displaying any prompts\.

```
$ eb create dev-env
Creating application version archive "app-160312_014028".
Uploading test/app-160312_014028.zip to S3. This may take a while.
Upload Complete.
Application test has been created.
Environment details for: dev-env
  Application name: test
  Region: us-east-2
  Deployed Version: app-160312_014028
  Environment ID: e-6fgpkjxyyi
  Platform: 64bit Amazon Linux 2015.09 v2.0.8 running PHP 5.6
  Tier: WebServer-Standard
  CNAME: UNKNOWN
  Updated: 2016-03-12 01:40:33.614000+00:00
Printing Status:
...
```

The following command creates an environment in a custom VPC\.

```
$ eb create dev-vpc --vpc.id vpc-0ce8dd99 --vpc.elbsubnets subnet-b356d7c6,subnet-02f74b0c --vpc.ec2subnets subnet-0bb7f0cd,subnet-3b6697c1 --vpc.securitygroup sg-70cff265
Creating application version archive "app-160312_014309".
Uploading test/app-160312_014309.zip to S3. This may take a while.
Upload Complete.
Environment details for: dev-vpc
  Application name: test
  Region: us-east-2
  Deployed Version: app-160312_014309
  Environment ID: e-pqkcip3mns
  Platform: 64bit Amazon Linux 2015.09 v2.0.8 running Java 8
  Tier: WebServer-Standard
  CNAME: UNKNOWN
  Updated: 2016-03-12 01:43:14.057000+00:00
Printing Status:
...
```