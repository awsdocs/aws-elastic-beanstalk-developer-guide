# Setting Configuration Options During Environment Creation<a name="environment-configuration-methods-during"></a>

When you create an AWS Elastic Beanstalk environment by using the AWS Management Console, EB CLI, AWS CLI, an SDK, or the Elastic Beanstalk API, you can provide values for configuration options to customize your environment and the AWS resources that are launched within it\.

For anything other than a one\-off configuration change, you can store configuration files locally, in your source bundle, or in Amazon S3\.

This topic includes procedures for all of the methods of setting configuration options during environment creation\.


+ [In the AWS Management Console](#configuration-options-during-console)
+ [With the EB CLI](#configuration-options-during-ebcli)
+ [With the AWS CLI](#configuration-options-during-awscli)

## In the AWS Management Console<a name="configuration-options-during-console"></a>

When you create an Elastic Beanstalk environment in the AWS Management Console, you can provide configuration options using configuration files, saved configurations, and forms in the **Create New Environment** wizard\.


+ [Using Configuration Files \(`.ebextensions`\)](#configuration-options-during-console-ebextensions)
+ [Using a Saved Configuration](#configuration-options-during-console-savedconfig)
+ [Using the New Environment Wizard](#configuration-options-during-console-wizard)

### Using Configuration Files \(`.ebextensions`\)<a name="configuration-options-during-console-ebextensions"></a>

Include `.config` files in your application source bundle in a folder named `.ebextensions`\.

```
~/workspace/my-app-v1.zip
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- index.php
`-- styles.css
```

Upload the source bundle to Elastic Beanstalk normally during environment creation\.

The Elastic Beanstalk console applies recommended values for some configuration options and has form fields for others\. Options configured by the Elastic Beanstalk console are applied directly to the environment and override settings in configuration files\.

### Using a Saved Configuration<a name="configuration-options-during-console-savedconfig"></a>

When you create a new environment using the Elastic Beanstalk console, one of the first steps is to choose a configuration\. The configuration can be a **Predefined configuration**, typically the latest version of a platform such as **PHP** or **Tomcat**, or it can be a **Saved configuration**\.

**To apply a saved configuration during environment creation \(AWS Management Console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. Choose **Saved Configurations**\.

1. Choose a saved configuration and then choose **Launch environment**\.

1. Proceed through the wizard to create your environment\.

Saved configurations are application specific\. See  for details on creating saved configurations\.

### Using the New Environment Wizard<a name="configuration-options-during-console-wizard"></a>

Most of the standard configuration options are presented on the **Configuration Details** and **Permissions** pages of the wizard\. If you create an Amazon RDS database or configure a VPC for your environment, additional configuration options are available on the pages for those resources\.

**To set configuration options during environment creation \(AWS Management Console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose or create an application\.

1. Choose **Create New Environment**\.

1. Proceed through the wizard to the **Configuration Details** page\.

1. Fill in the fields on this page to set the corresponding configuration options\.

1. Proceed through the wizard to create your environment\.

Any options that you set in the new environment wizard are set directly on the environment and override any option settings in saved configurations or configuration files \(`.ebextensions`\) that you apply\. You can remove settings after the environment is created using the EB CLI or AWS CLI to allow the settings in saved configurations or configuration files to surface\.

## With the EB CLI<a name="configuration-options-during-ebcli"></a>


+ [Using Configuration Files \(`.ebextensions`\)](#configuration-options-during-ebcli-ebextensions)
+ [Using Saved Configurations](#configuration-options-during-ebcli-savedconfig)
+ [Using Command Line Options](#configuration-options-during-ebcli-params)

### Using Configuration Files \(`.ebextensions`\)<a name="configuration-options-during-ebcli-ebextensions"></a>

Include `.config` files in your project folder under `.ebextensions` to deploy them with your application code\.

```
~/workspace/my-app/
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- .elasticbeanstalk
|   `-- config.yml
|-- index.php
`-- styles.css
```

Create your environment and deploy your source code to it with `eb create`:

```
~/workspace/my-app$ eb create my-env
```

### Using Saved Configurations<a name="configuration-options-during-ebcli-savedconfig"></a>

To apply a saved configuration when you create an environment with `eb create`, use the `--cfg` option:

```
~/workspace/my-app$ eb create --cfg savedconfig
```

The saved configuration can be stored in your project folder or in your Elastic Beanstalk storage location on Amazon S3\. In the above example, the EB CLI first looks for a saved configuration file named `savedconfig.cfg.yml` in the folder `.elasticbeanstalk/saved_configs/`\. Do not include the file extensions \(`.cfg.yml`\) when applying a saved configuration with `--cfg`\.

```
~/workspace/my-app/
|-- .ebextensions
|   `-- healthcheckurl.config
|-- .elasticbeanstalk
|   |-- saved_configs
|   |   `-- savedconfig.cfg.yml
|   `-- config.yml
|-- index.php
`-- styles.css
```

If the EB CLI does not find the configuration locally, it looks in the Elastic Beanstalk storage location in Amazon S3\. For details on creating, editing and uploading saved configurations, see \.

### Using Command Line Options<a name="configuration-options-during-ebcli-params"></a>

The EB CLI `eb create` command has several options that you can use to set configuration options during environment creation\. These options can be used to add an RDS database to your environment, configure a VPC, or override recommended values\.

For example, the EB CLI will use the `t2.micro` instance type by default\. To choose a different instance type, use the `--instance_type` option:

```
$ eb create my-env --instance_type t2.medium
```

To create an Amazon RDS database instance and attach it to your environment, use the `--database` options:

```
$ eb create --database.engine postgres --database.username dbuser
```

If you leave out the environment name, database password or any other parameters that are required to create your environment, the EB CLI prompts you to enter them\.

See eb create for a full list of available options and usage examples\.

## With the AWS CLI<a name="configuration-options-during-awscli"></a>

When you use the `create-environment` command to create an Elastic Beanstalk environment with the AWS CLI, the AWS CLI does not apply any recommended values\. All configuration options defined in configuration files in the source bundle that you specify 


+ [Using Configuration Files \(`.ebextensions`\)](#configuration-options-during-awscli-ebextensions)
+ [Using a Saved Configuration](#configuration-options-during-awscli-savedconfig)
+ [Using Command Line Options](#configuration-options-during-awscli-params)

### Using Configuration Files \(`.ebextensions`\)<a name="configuration-options-during-awscli-ebextensions"></a>

To apply configuration files to an environment that you create with the AWS CLI, include them in the application source bundle that you upload to Amazon S3\.

```
~/workspace/my-app-v1.zip
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- index.php
`-- styles.css
```

**To upload an application source bundle and create an environment with the AWS CLI**

1. If you don't already have an Elastic Beanstalk bucket in Amazon S3, create one with `create-storage-location`:

   ```
   $ aws elasticbeanstalk create-storage-location
   {
       "S3Bucket": "elasticbeanstalk-us-west-2-0123456789012"
   }
   ```

1. Upload your application source bundle to Amazon S3:

   ```
   $ aws s3 cp sourcebundle.zip s3://elasticbeanstalk-us-west-2-0123456789012/my-app/sourcebundle.zip
   ```

1. Create the application version:

   ```
   $ aws elasticbeanstalk create-application-version --application-name my-app --version-label v1 --description MyAppv1 --source-bundle S3Bucket="elasticbeanstalk-us-west-2-0123456789012",S3Key="my-app/sourcebundle.zip" --auto-create-application
   ```

1. Create the environment:

   ```
   $ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --solution-stack-name "64bit Amazon Linux 2015.03 v2.0.0 running Tomcat 8 Java 8"
   ```

### Using a Saved Configuration<a name="configuration-options-during-awscli-savedconfig"></a>

To apply a saved configuration to an environment during creation, use the `--template-name` parameter:

```
$ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --template-name savedconfig --version-label v1
```

When you specify a saved configuration, do not also specify a solution stack name\. Saved configurations already specify a solution stack and Elastic Beanstalk will return an error if you try to use both options\. 

### Using Command Line Options<a name="configuration-options-during-awscli-params"></a>

Use the `--option-settings` parameter to specify configuration options in JSON format:

```
$ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --template-name savedconfig --option-settings '[
  {
    "Namespace": "aws:elasticbeanstalk:application",
    "OptionName": "Application Healthcheck URL",
    "Value": "/health"
  }
]
```

To load the JSON from a file, use the `file://` prefix:

```
$ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --template-name savedconfig --option-settings file://healthcheckurl.json
```

Elastic Beanstalk applies option settings that you specify with the `--option-settings` option directly to your environment\. If the same options are specified in a saved configuration or configuration file, `--option-settings` overrides those values\.