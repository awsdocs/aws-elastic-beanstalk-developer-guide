# Setting configuration options during environment creation<a name="environment-configuration-methods-during"></a>

When you create an AWS Elastic Beanstalk environment by using the Elastic Beanstalk console, EB CLI, AWS CLI, an SDK, or the Elastic Beanstalk API, you can provide values for configuration options to customize your environment and the AWS resources that are launched within it\.

For anything other than a one\-off configuration change, you can [store configuration files](environment-configuration-methods-before.md) locally, in your source bundle, or in Amazon S3\.

This topic includes procedures for all of the methods to set configuration options during environment creation\.

**Topics**
+ [In the Elastic Beanstalk console](#configuration-options-during-console)
+ [Using the EB CLI](#configuration-options-during-ebcli)
+ [Using the AWS CLI](#configuration-options-during-awscli)

## In the Elastic Beanstalk console<a name="configuration-options-during-console"></a>

When you create an Elastic Beanstalk environment in the Elastic Beanstalk console, you can provide configuration options using configuration files, saved configurations, and forms in the **Create New Environment** wizard\.

**Topics**
+ [Using configuration files \(`.ebextensions`\)](#configuration-options-during-console-ebextensions)
+ [Using a saved configuration](#configuration-options-during-console-savedconfig)
+ [Using the new environment wizard](#configuration-options-during-console-wizard)

### Using configuration files \(`.ebextensions`\)<a name="configuration-options-during-console-ebextensions"></a>

Include `.config` files in your [application source bundle](applications-sourcebundle.md) in a folder named `.ebextensions`\.

For details about configuration files, see [\.Ebextensions](ebextensions.md)\.

```
~/workspace/my-app-v1.zip
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- index.php
`-- styles.css
```

Upload the source bundle to Elastic Beanstalk normally, during [environment creation](using-features.environments.md)\.

The Elastic Beanstalk console applies [recommended values](command-options.md#configuration-options-recommendedvalues) for some configuration options and has form fields for others\. Options configured by the Elastic Beanstalk console are applied directly to the environment and override settings in configuration files\.

### Using a saved configuration<a name="configuration-options-during-console-savedconfig"></a>

When you create a new environment using the Elastic Beanstalk console, one of the first steps is to choose a configuration\. The configuration can be a [**predefined configuration**](concepts.platforms.md), typically the latest version of a platform such as **PHP** or **Tomcat**, or it can be a **saved configuration**\.

**To apply a saved configuration during environment creation \(Elastic Beanstalk console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose your application's name from the list\.
**Note**  
If you have many applications, use the search bar to filter the application list\.

1. In the navigation pane, find your application's name and choose **Saved configurations**\.

1. Select the saved configuration you want to apply, and then choose **Launch environment**\.

1. Proceed through the wizard to create your environment\.

Saved configurations are application\-specific\. See [Saved configurations](environment-configuration-methods-before.md#configuration-options-before-savedconfig) for details on creating saved configurations\.

### Using the new environment wizard<a name="configuration-options-during-console-wizard"></a>

Most of the standard configuration options are presented on the **Configure more options** page of the [Create New Environment wizard](environments-create-wizard.md)\. If you create an Amazon RDS database or configure a VPC for your environment, additional configuration options are available for those resources\.

**To set configuration options during environment creation \(Elastic Beanstalk console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**\.

1. Choose or [create](applications.md) an application\.

1. Choose **Actions**, and then choose **Create environment**\.

1. Proceed through the wizard, and choose **Configure more options**\.

1. Choose any of the **configuration presets**, and then choose **Edit** in one or more of the configuration categories to change a group of related configuration options\.

1. When you are done making option selections, choose **Create environment**\.

Any options that you set in the new environment wizard are set directly on the environment and override any option settings in saved configurations or configuration files \(`.ebextensions`\) that you apply\. You can remove settings after the environment is created using the [EB CLI](environment-configuration-methods-after.md#configuration-options-after-ebcli) or [AWS CLI](environment-configuration-methods-after.md#configuration-options-after-awscli) to allow the settings in saved configurations or configuration files to surface\.

For details about the new environment wizard, see [The create new environment wizard](environments-create-wizard.md)\.

## Using the EB CLI<a name="configuration-options-during-ebcli"></a>

**Topics**
+ [Using configuration files \(`.ebextensions`\)](#configuration-options-during-ebcli-ebextensions)
+ [Using saved configurations](#configuration-options-during-ebcli-savedconfig)
+ [Using command line options](#configuration-options-during-ebcli-params)

### Using configuration files \(`.ebextensions`\)<a name="configuration-options-during-ebcli-ebextensions"></a>

Include `.config` files in your project folder under `.ebextensions` to deploy them with your application code\.

For details about configuration files, see [\.Ebextensions](ebextensions.md)\.

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

Create your environment and deploy your source code to it with eb create\.

```
~/workspace/my-app$ eb create my-env
```

### Using saved configurations<a name="configuration-options-during-ebcli-savedconfig"></a>

To apply a saved configuration when you create an environment with [eb create](eb3-create.md), use the `--cfg` option\.

```
~/workspace/my-app$ eb create --cfg savedconfig
```

You can store the saved configuration in your project folder or in your Elastic Beanstalk storage location on Amazon S3\. In the previous example, the EB CLI first looks for a saved configuration file named `savedconfig.cfg.yml` in the folder `.elasticbeanstalk/saved_configs/`\. Do not include the file name extensions \(`.cfg.yml`\) when applying a saved configuration with `--cfg`\.

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

If the EB CLI does not find the configuration locally, it looks in the Elastic Beanstalk storage location in Amazon S3\. For details on creating, editing, and uploading saved configurations, see [Saved configurations](environment-configuration-methods-before.md#configuration-options-before-savedconfig)\.

### Using command line options<a name="configuration-options-during-ebcli-params"></a>

The EB CLI eb create command has several [options](eb3-create.md#eb3-createoptions) that you can use to set configuration options during environment creation\. You can use these options to add an RDS database to your environment, configure a VPC, or override [recommended values](command-options.md#configuration-options-recommendedvalues)\.

For example, the EB CLI uses the `t2.micro` instance type by default\. To choose a different instance type, use the `--instance_type` option\.

```
$ eb create my-env --instance_type t2.medium
```

To create an Amazon RDS database instance and attach it to your environment, use the `--database` options\.

```
$ eb create --database.engine postgres --database.username dbuser
```

If you leave out the environment name, database password, or any other parameters that are required to create your environment, the EB CLI prompts you to enter them\.

See [eb create](eb3-create.md) for a full list of available options and usage examples\.

## Using the AWS CLI<a name="configuration-options-during-awscli"></a>

When you use the `create-environment` command to create an Elastic Beanstalk environment with the AWS CLI, the AWS CLI does not apply any [recommended values](command-options.md#configuration-options-recommendedvalues)\. All configuration options are defined in configuration files in the source bundle that you specify\. 

**Topics**
+ [Using configuration files \(`.ebextensions`\)](#configuration-options-during-awscli-ebextensions)
+ [Using a saved configuration](#configuration-options-during-awscli-savedconfig)
+ [Using command line options](#configuration-options-during-awscli-params)

### Using configuration files \(`.ebextensions`\)<a name="configuration-options-during-awscli-ebextensions"></a>

To apply configuration files to an environment that you create with the AWS CLI, include them in the application source bundle that you upload to Amazon S3\.

For details about configuration files, see [\.Ebextensions](ebextensions.md)\.

```
~/workspace/my-app-v1.zip
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- index.php
`-- styles.css
```

**To upload an application source bundle and create an environment with the AWS CLI**

1. If you don't already have an Elastic Beanstalk bucket in Amazon S3, create one with `create-storage-location`\.

   ```
   $ aws elasticbeanstalk create-storage-location
   {
       "S3Bucket": "elasticbeanstalk-us-west-2-123456789012"
   }
   ```

1. Upload your application source bundle to Amazon S3\.

   ```
   $ aws s3 cp sourcebundle.zip s3://elasticbeanstalk-us-west-2-123456789012/my-app/sourcebundle.zip
   ```

1. Create the application version\.

   ```
   $ aws elasticbeanstalk create-application-version --application-name my-app --version-label v1 --description MyAppv1 --source-bundle S3Bucket="elasticbeanstalk-us-west-2-123456789012",S3Key="my-app/sourcebundle.zip" --auto-create-application
   ```

1. Create the environment\.

   ```
   $ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --solution-stack-name "64bit Amazon Linux 2015.03 v2.0.0 running Tomcat 8 Java 8"
   ```

### Using a saved configuration<a name="configuration-options-during-awscli-savedconfig"></a>

To apply a saved configuration to an environment during creation, use the `--template-name` parameter\.

```
$ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --template-name savedconfig --version-label v1
```

When you specify a saved configuration, do not also specify a solution stack name\. Saved configurations already specify a solution stack and Elastic Beanstalk will return an error if you try to use both options\. 

### Using command line options<a name="configuration-options-during-awscli-params"></a>

Use the `--option-settings` parameter to specify configuration options in JSON format\.

```
$ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --template-name savedconfig --option-settings '[
  {
    "Namespace": "aws:elasticbeanstalk:application",
    "OptionName": "Application Healthcheck URL",
    "Value": "/health"
  }
]
```

To load the JSON from a file, use the `file://` prefix\.

```
$ aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --template-name savedconfig --option-settings file://healthcheckurl.json
```

Elastic Beanstalk applies option settings that you specify with the `--option-settings` option directly to your environment\. If the same options are specified in a saved configuration or configuration file, `--option-settings` overrides those values\.