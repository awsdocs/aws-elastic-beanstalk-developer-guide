# Platform script tools<a name="custom-platforms-scripts"></a>

This topic describes tools that AWS Elastic Beanstalk provides on instances of environments that use Amazon Linux platform versions\. You can use these tools to enhance platform hook scripts that run on\-instance in your environment\.

## get\-config<a name="custom-platforms-scripts.get-config"></a>

Use the `get-config` tool to retrieve environment variable values and other platform and instance information\. The tool is available at `/opt/elasticbeanstalk/bin/get-config`\.

### get\-config commands<a name="custom-platforms-scripts.get-config.commands"></a>

Each `get-config` tool command returns a specific type of information\. Use the following syntax to run any of the tool's commands\.

```
$ /opt/elasticbeanstalk/bin/get-config command [ options ]
```

The following example runs the `environment` command\.

```
$ /opt/elasticbeanstalk/bin/get-config environment -k PORT
```

Depending on the command and options you choose, the tool returns an object \(JSON or YAML\) with key\-value pairs, or a single value\.

You can test `get-config` by using SSH to connect to an instance in your Elastic Beanstalk environment\.

**Note**  
When you run `get-config` for testing, some commands might require root user privileges to access the underlying information\. If you get an access permission error, run the command again under `sudo`\.  
You don't need to add `sudo` when using the tool in the scripts that you deploy to your environment\. Elastic Beanstalk runs all your scripts as the root user\.

The following sections describe the tool's commands\.

#### optionsettings – Configuration options<a name="custom-platforms-scripts.get-config.commands.optionsettings"></a>

The `get-config optionsettings` command returns an object listing the configuration options that are set on the environment and used by the platform on environment instances\. They are organized by namespace\.

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

The `get-config environment` command returns an object containing a list of environment properties\. These include both user\-configured properties and those provided by Elastic Beanstalk\.

```
$ /opt/elasticbeanstalk/bin/get-config environment
{"JDBC_CONNECTION_STRING":"","RDS_PORT":"3306","RDS_HOSTNAME":"anj9aw1b0tbj6b.cijbpanmxz5u.us-west-2.rds.amazonaws.com","RDS_USERNAME":"testusername","RDS_DB_NAME":"ebdb","RDS_PASSWORD":"testpassword1923851"}
```

For example, Elastic Beanstalk provides environment properties for connecting to an integrated Amazon RDS DB instance \(`RDS_HOSTNAME`, and so on\)\. These RDS connection properties appear in the output of `get-config environment`, but not in the output of `get-config optionsettings`, because they were not set in configuration options\.

To return a specific environment property, use the `--key` \(`-k`\) option to specify a property key\.

```
$ /opt/elasticbeanstalk/bin/get-config environment -k TESTPROPERTY
testvalue
```

#### container – On\-instance configuration values<a name="custom-platforms-scripts.get-config.commands.container"></a>

The `get-config container` command returns an object that lists platform and environment configuration values as they are reflected on environment instances\. 

The following example shows the command's output on an Amazon Linux 2 Tomcat environment\.

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

The `get-config addons` command returns an object containing configuration information of environment add\-ons\. Use it to retrieve the configuration of an Amazon RDS database associated with the environment\.

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

The `get-config platformconfig` command returns an object containing platform configuration information that is constant to the platform version\. Output is the same on all environments running the same platform version\. The command's output object has two embedded objects:
+ `GeneralConfig` – Contains information that is constant across the latest versions of all Amazon Linux 2 platform branches\.
+ `PlatformSpecificConfig` – Contains information that is constant for the platform version and is specific to it\.

The following example shows the command's output on an environment that uses the *Tomcat 8\.5 running Corretto 11* platform branch\.

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

Use the `--output` option to specify the output object format\. Valid values are `JSON` \(the default\) and `YAML`\. This is a global option and you have to specify it before the command name\.

The following example returns configuration option values in YAML format\.

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

## download\-source\-bundle \(Amazon Linux AMI only\)<a name="custom-platforms-scripts.download"></a>

On Amazon Linux AMI platform branches \(preceding Amazon Linux 2\), Elastic Beanstalk provides an additional tool, `download-source-bundle`\. Use it to download your application source code during the deployment of your platform\. The tool is available at `/opt/elasticbeanstalk/bin/download-source-bundle`\.

The example script `00-unzip.sh` is located in the `appdeploy/pre` folder on environment instances\. It demonstrates using `download-source-bundle` to download application source code to the `/opt/elasticbeanstalk/deploy/appsource` folder during deployment\.