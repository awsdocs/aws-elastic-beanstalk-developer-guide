# Platform script tools<a name="custom-platforms-scripts"></a>

This topic describes tools that AWS Elastic Beanstalk provides for environments that use Amazon Linux platforms\. The tools are located on the Amazon EC2 instances of the Elastic Beanstalk environments\. 

## get\-config<a name="custom-platforms-scripts.get-config"></a>

Use the `get-config` tool to retrieve environment variable values and other platform and instance information\. The tool is available at `/opt/elasticbeanstalk/bin/get-config`\.

### get\-config commands<a name="custom-platforms-scripts.get-config.commands"></a>

Each `get-config` tool command returns a specific type of information\. Use the following syntax to run the commands of any of the tools\.

```
$ /opt/elasticbeanstalk/bin/get-config command [ options ]
```

The following example runs the `environment` command\.

```
$ /opt/elasticbeanstalk/bin/get-config environment -k PORT
```

Depending on the command and options you choose, the tool returns an object \(JSON or YAML\) with key\-value pairs or a single value\.

You can test `get-config` by using SSH to connect to an EC2 instance in your Elastic Beanstalk environment\.

**Note**  
When you run `get-config` for testing, some commands might require root user privileges to access the underlying information\. If you get an access permission error, run the command again under `sudo`\.  
You don't need to add `sudo` when using the tool in the scripts that you deploy to your environment\. Elastic Beanstalk runs all your scripts as the root user\.

The following sections describe the commands for the tools\.

#### optionsettings – Configuration options<a name="custom-platforms-scripts.get-config.commands.optionsettings"></a>

The `get-config optionsettings` command returns an object that's listing the configuration options that are set on the environment and used by the platform on environment instances\. They're organized by namespace\.

```
$ /opt/elasticbeanstalk/bin/get-config optionsettings
{"aws:elasticbeanstalk:application:environment":{"JDBC_CONNECTION_STRING":""},"aws:elasticbeanstalk:container:tomcat:jvmoptions":{"JVM Options":"","Xms":"256m","Xmx":"256m"},"aws:elasticbeanstalk:environment:proxy":{"ProxyServer":"nginx","StaticFiles":[""]},"aws:elasticbeanstalk:healthreporting:system":{"SystemType":"enhanced"},"aws:elasticbeanstalk:hostmanager":{"LogPublicationControl":"false"}}
```

To return a specific configuration option value, use the `--namespace` \(`-n`\) option to specify a namespace, and the `--option-name` \(`-o`\) option to specify an option name\.

```
$ /opt/elasticbeanstalk/bin/get-config optionsettings -n aws:elasticbeanstalk:container:php:phpini -o memory_limit
256M
```

#### environment – Environment properties<a name="custom-platforms-scripts.get-config.commands.environment"></a>

The `get-config environment` command returns an object containing a list of environment properties\. These include both user\-configured properties and those that are provided by Elastic Beanstalk\.

```
$ /opt/elasticbeanstalk/bin/get-config environment
{"JDBC_CONNECTION_STRING":"","RDS_PORT":"3306","RDS_HOSTNAME":"anj9aw1b0tbj6b.cijbpanmxz5u.us-west-2.rds.amazonaws.com","RDS_USERNAME":"testusername","RDS_DB_NAME":"ebdb","RDS_PASSWORD":"testpassword1923851"}
```

For example, Elastic Beanstalk provides environment properties for connecting to an integrated Amazon RDS DB instance \(for example, `RDS_HOSTNAME`\)\. These RDS connection properties appear in the output of `get-config environment`\. However, they don't appear in the output of `get-config optionsettings`\. This is because they weren't set in configuration options\.

To return a specific environment property, use the `--key` \(`-k`\) option to specify a property key\.

```
$ /opt/elasticbeanstalk/bin/get-config environment -k TESTPROPERTY
testvalue
```

#### container – On\-instance configuration values<a name="custom-platforms-scripts.get-config.commands.container"></a>

The `get-config container` command returns an object that lists platform and environment configuration values for environment instances\. 

The following example shows the output for the command on an Amazon Linux 2 Tomcat environment\.

```
$ /opt/elasticbeanstalk/bin/get-config container
{"common_log_list":["/var/log/eb-engine.log","/var/log/eb-hooks.log"],"default_log_list":["/var/log/nginx/access.log","/var/log/nginx/error.log"],"environment_name":"myenv-1da84946","instance_port":"80","log_group_name_prefix":"/aws/elasticbeanstalk","proxy_server":"nginx","static_files":[""],"xray_enabled":"false"}
```

To return the value of a specific key, use the `--key` \(`-k`\) option to specify the key\.

```
$ /opt/elasticbeanstalk/bin/get-config container -k environment_name
myenv-1da84946
```

#### addons – Add\-on configuration values<a name="custom-platforms-scripts.get-config.commands.addons"></a>

The `get-config addons` command returns an object that contains configuration information of environment add\-ons\. Use it to retrieve the configuration of an Amazon RDS database that's associated with the environment\.

```
$ /opt/elasticbeanstalk/bin/get-config addons
{"rds":{"Description":"RDS Environment variables","env":{"RDS_DB_NAME":"ebdb","RDS_HOSTNAME":"ea13k2wimu1dh8i.c18mnpu5rwvg.us-east-2.rds.amazonaws.com","RDS_PASSWORD":"password","RDS_PORT":"3306","RDS_USERNAME":"user"}}}
```

You can restrict the result in two ways\. To retrieve values for a specific add\-on, use the `--add-on` \(`-a`\) option to specify the add\-on name\.

```
$ /opt/elasticbeanstalk/bin/get-config addons -a rds
{"Description":"RDS Environment variables","env":{"RDS_DB_NAME":"ebdb","RDS_HOSTNAME":"ea13k2wimu1dh8i.c18mnpu5rwvg.us-east-2.rds.amazonaws.com","RDS_PASSWORD":"password","RDS_PORT":"3306","RDS_USERNAME":"user"}}
```

To return the value of a specific key within an add\-on, add the `--key` \(`-k`\) option to specify the key\.

```
$ /opt/elasticbeanstalk/bin/get-config addons -a rds -k RDS_DB_NAME
ebdb
```

#### platformconfig – Constant configuration values<a name="custom-platforms-scripts.get-config.commands.platformconfig"></a>

The `get-config platformconfig` command returns an object that contains platform configuration information that's constant to the platform version\. The output is the same on all environments that run the same platform version\. The output object for the command has two embedded objects:
+ `GeneralConfig` – Contains information that's constant across the latest versions of all Amazon Linux 2 platform branches\.
+ `PlatformSpecificConfig` – Contains information that's constant for the platform version and is specific to it\.

The following example shows the output for the command on an environment that uses the *Tomcat 8\.5 running Corretto 11* platform branch\.

```
$ /opt/elasticbeanstalk/bin/get-config platformconfig
{"GeneralConfig":{"AppUser":"webapp","AppDeployDir":"/var/app/current/","AppStagingDir":"/var/app/staging/","ProxyServer":"nginx","DefaultInstancePort":"80"},"PlatformSpecificConfig":{"ApplicationPort":"8080","JavaVersion":"11","TomcatVersion":"8.5"}}
```

To return the value of a specific key, use the `--key` \(`-k`\) option to specify the key\. These keys are unique across the two embedded objects\. You don't need to specify the object that contains the key\.

```
$ /opt/elasticbeanstalk/bin/get-config platformconfig -k AppStagingDir
/var/app/staging/
```

### get\-config output options<a name="custom-platforms-scripts.get-config.global"></a>

Use the `--output` option to specify the output object format\. Valid values are `JSON` \(default\) and `YAML`\. This is a global option\. You must specify it before the command name\.

The following example returns configuration option values in the YAML format\.

```
$ /opt/elasticbeanstalk/bin/get-config --output YAML optionsettings
aws:elasticbeanstalk:application:environment:
  JDBC_CONNECTION_STRING: ""
aws:elasticbeanstalk:container:tomcat:jvmoptions:
  JVM Options: ""
  Xms: 256m
  Xmx: 256m
aws:elasticbeanstalk:environment:proxy:
  ProxyServer: nginx
  StaticFiles:
        - ""
aws:elasticbeanstalk:healthreporting:system:
  SystemType: enhanced
aws:elasticbeanstalk:hostmanager:
  LogPublicationControl: "false"
```

## pkg\-repo<a name="custom-platforms-scripts.pkg-repo"></a>

In some urgent circumstances, you might need to update your Amazon EC2 instances with an Amazon Linux 2 security patch that hasn't yet been released with the required Elastic Beanstalk platform versions\. You can't perform a manual update on your Elastic Beanstalk environments by default\. This is because the platform versions are locked to a specific version of the Amazon Linux 2 repository\. This lock ensures that instances run supported and consistent software versions\. For urgent cases, the `pkg-repo` tool allows a workaround to manually update yum packages on Amazon Linux 2 if you need to install it on an environment before it's released in a new Elastic Beanstalk platform version\.

The `pkg-repo` tool on Amazon Linux 2 platforms provides the capability to unlock the `yum` package repositories\. You can then manually perform a yum update for a security patch\. Conversely, you can follow the update by using the tool to lock the yum package repositories to prevent further updates\. The `pkg-repo` tool is available at the `/opt/elasticbeanstalk/bin/pkg-repo` directory of all the EC2 instances in your Elastic Beanstalk environments\.

Changes using the `pkg-repo` tool are made only on the EC2 instance that the tool is used on\. They don’t affect other instances or prevent future updates to the environment\. The examples that are provided later in this topic explain how to apply the changes across all instances by calling the `pkg-repo` commands from scripts and configuration files\.

**Warning**  
We don't recommend this tool for most users\. Any manual changes applied to an unlocked platform version are considered out of band\. This option is only viable for those users in urgent circumstances that can accept the following risks:  
Platforms versions can't be guaranteed to be consistent across all instances in your environments\.
Environments that are modified using the `pkg-repo` tool aren't guaranteed to function properly\. They haven't been tested and verified on Elastic Beanstalk supported platforms\.
We strongly recommend applying best practices that include testing and backout plans\. To help facilitate best practices, you can use the Elastic Beanstalk console and EB CLI to clone an environment and swap environment URLs\. For more information about using these operations, see [Blue/Green deployments](using-features.CNAMESwap.md) in the *Managing environments* chapter of this guide\.

If you plan to manually edit yum repository configuration files, run the `pkg-repo` tool first\. The `pkg-repo` tool might not work as intended in an Amazon Linux 2 environment with manually edited yum repository configuration files\. This is because the tool might not recognize the configuration changes\.

For more information about the Amazon Linux package repository, see the [Package repository](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-basics.html#package-repository) topic in the *Amazon EC2 User Guide for Linux Instances*\.

### pkg\-repo commands<a name="custom-platforms-scripts.pkg-repo.commands"></a>

Use the following syntax to run the `pkg-repo` tool commands\.

```
$ /opt/elasticbeanstalk/bin/pkg-repo command [options]
```

The `pkg-repo` commands are the following:
+ lock – locks the `yum` package repositories to a specific version
+ unlock – unlocks the `yum` package repositories from a specific version
+ status – lists all the `yum` package repositories and their current lock status
+ help – shows general help or help for one command

The options apply to the commands as follows:
+ `lock`, `unlock` and `status ` – options: `-h`, `--help`, or none \(default\)\.
+ `help` – options: `lock`, `unlock`, `status`, or none \(default\)\.



The following example runs the unlock command\.

```
$ sudo /opt/elasticbeanstalk/bin/pkg-repo unlock
Amazon Linux 2 core package repo successfully unlocked
Amazon Linux 2 extras package repo successfully unlocked
```

The following example runs the lock command\.

```
$ sudo /opt/elasticbeanstalk/bin/pkg-repo lock
Amazon Linux 2 core package repo successfully locked
Amazon Linux 2 extras package repo successfully locked
```

The following example runs the status command\.

```
$ sudo /opt/elasticbeanstalk/bin/pkg-repo status
Amazon Linux 2 core package repo is currently UNLOCKED
Amazon Linux 2 extras package repo is currently UNLOCKED
```

The following example runs the help command for the lock command\.

```
$ sudo /opt/elasticbeanstalk/bin/pkg-repo help lock
```

The following example runs the help command for the `pkg-repo` tool\.

```
$ sudo /opt/elasticbeanstalk/bin/pkg-repo help
```

You can test `pkg-repo` by using SSH to connect to an instance in your Elastic Beanstalk environment\. One SSH option is the EB CLI [eb ssh](eb3-ssh.md) command\.

**Note**  
The `pkg-repo` tool requires root user privileges to run\. If you get an access permission error, run the command again under `sudo`\.  
You don't need to add `sudo` when using the tool in the scripts or configuration files that you deploy to your environment\. Elastic Beanstalk runs all your scripts as the root user\.

### pkg\-repo examples<a name="custom-platforms-scripts.pkg-repo.examples"></a>

The previous section provides command line examples for testing on an individual EC2 instance of an Elastic Beanstalk environment\. This approach can be helpful for testing\. However, it updates only one instance at a time, so it isn’t practical for applying changes to all of the instances in an environment\.

A more pragmatic approach is to use [platform hook](platforms-linux-extend.md#platforms-linux-extend.hooks) scripts or an [`.ebextensions`](ebextensions.md) configuration file to apply the changes across all instances in a consistent manner\.

The following example calls `pkg-repo` from a configuration file in the [`.ebextensions`](ebextensions.md) folder\. Elastic Beanstalk runs the commands in the `update_package.config` file when you deploy your application source bundle\.

```
.ebextensions
└── update_package.config
```

To receive the latest version of the *docker* package, this configuration specifies the *docker* package in the yum update command\.

```
### update_package.config ###

commands:
  update_package:
    command: |
      /opt/elasticbeanstalk/bin/pkg-repo unlock
      yum update docker -y
      /opt/elasticbeanstalk/bin/pkg-repo lock
      yum clean all -y
      rm -rf /var/cache/yum
```

This configuration doesn't specify any packages in the yum update command\. All available updates are applied as a result\.

```
### update_package.config ###

commands:
  update_package:
    command: |
      /opt/elasticbeanstalk/bin/pkg-repo unlock
      yum update -y
      /opt/elasticbeanstalk/bin/pkg-repo lock
      yum clean all -y
      rm -rf /var/cache/yum
```

The following example calls `pkg-repo` from a bash script as a [platform hook](platforms-linux-extend.md#platforms-linux-extend.hooks)\. Elastic Beanstalk runs the `update_package.sh` script file that's located in the `prebuild` subdirectory\.

```
.platform
└── hooks
    └── prebuild
        └── update_package.sh
```

To receive the latest version of the *docker* package, this script specifies the *docker* package in the yum update command\. If the package name is omitted, all the available updates are applied\. The prior configuration file example demonstrates this\.

```
### update_package.sh ###

#!/bin/bash

/opt/elasticbeanstalk/bin/pkg-repo unlock
yum update docker -y
/opt/elasticbeanstalk/bin/pkg-repo lock
yum clean all -y
rm -rf /var/cache/yum
```

## download\-source\-bundle \(Amazon Linux AMI only\)<a name="custom-platforms-scripts.download"></a>

On Amazon Linux AMI platform branches \(preceding Amazon Linux 2\), Elastic Beanstalk provides an additional tool, which is `download-source-bundle`\. Use this tool to download your application source code when deploying your platform\. The tool is available at `/opt/elasticbeanstalk/bin/download-source-bundle`\.

The example script `00-unzip.sh` is located in the `appdeploy/pre` folder on environment instances\. It demonstrates how to use `download-source-bundle` to download the application source code to the `/opt/elasticbeanstalk/deploy/appsource` folder during deployment\.