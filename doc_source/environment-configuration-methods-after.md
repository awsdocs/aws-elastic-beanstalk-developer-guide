# Setting configuration options after environment creation<a name="environment-configuration-methods-after"></a>

You can modify the option settings on a running environment by applying saved configurations, uploading a new source bundle with configuration files \(`.ebextensions`\), or using a JSON document\. The EB CLI and Elastic Beanstalk console also have client\-specific functionality for setting and updating configuration options\.

When you set or change a configuration option, you can trigger a full environment update, depending on the severity of the change\. For example, changes to options in the [`aws:autoscaling:launchconfiguration`](command-options-general.md#command-options-general-autoscalinglaunchconfiguration), such as `InstanceType`, require that the Amazon EC2 instances in your environment are reprovisioned\. This triggers a [rolling update](using-features.rollingupdates.md)\. Other configuration changes can be applied without any interruption or reprovisioning\.

You can remove option settings from an environment with EB CLI or AWS CLI commands\. Removing an option that has been set directly on an environment at an API level allows settings in configuration files, which are otherwise masked by settings applied directly to an environment, to surface and take effect\.

Settings in saved configurations and configuration files can be overridden by setting the same option directly on the environment with one of the other configuration methods\. However, these can only be removed completely by applying an updated saved configuration or configuration file\. When an option is not set in a saved configuration, in a configuration file, or directly on an environment, the default value applies, if there is one\. See [Precedence](command-options.md#configuration-options-precedence) for details\.

**Topics**
+ [The Elastic Beanstalk console](#configuration-options-after-console)
+ [The EB CLI](#configuration-options-after-ebcli)
+ [The AWS CLI](#configuration-options-after-awscli)

## The Elastic Beanstalk console<a name="configuration-options-after-console"></a>

You can update configuration option settings in the Elastic Beanstalk console by deploying an application source bundle that contains configuration files, applying a saved configuration, or modifying the environment directly with the **Configuration** page in the environment management console\.

**Topics**
+ [Using configuration files \(`.ebextensions`\)](#configuration-options-after-console-ebextensions)
+ [Using a saved configuration](#configuration-options-after-console-savedconfig)
+ [Using the Elastic Beanstalk console](#configuration-options-after-console-configpage)

### Using configuration files \(`.ebextensions`\)<a name="configuration-options-after-console-ebextensions"></a>

Update configuration files in your source directory, create a new source bundle, and deploy the new version to your Elastic Beanstalk environment to apply the changes\.

For details about configuration files, see [\.Ebextensions](ebextensions.md)\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Use the on\-screen dialog box to upload the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, you can choose the site URL to open your website in a new tab\.

Changes made to configuration files will not override option settings in saved configurations or settings applied directly to the environment at the API level\. See [Precedence](command-options.md#configuration-options-precedence) for details\.

### Using a saved configuration<a name="configuration-options-after-console-savedconfig"></a>

Apply a saved configuration to a running environment to apply option settings that it defines\.

**To apply a saved configuration to a running environment \(Elastic Beanstalk console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose your application's name from the list\.
**Note**  
If you have many applications, use the search bar to filter the application list\.

1. In the navigation pane, find your application's name and choose **Saved configurations**\.

1. Select the saved configuration you want to apply, and then choose **Load**\.

1. Select an environment, and then choose **Load**\.

Settings defined in a saved configuration override settings in configuration files, and are overridden by settings configured using the environment management console\.

See [Saved configurations](environment-configuration-methods-before.md#configuration-options-before-savedconfig) for details on creating saved configurations\.

### Using the Elastic Beanstalk console<a name="configuration-options-after-console-configpage"></a>

The Elastic Beanstalk console presents many configuration options on the **Configuration** page for each environment\.

**To change configuration options on a running environment \(Elastic Beanstalk console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. Find the configuration page you want to edit:
   + If you see the option you're interested in, or you know which configuration category it's in, choose **Edit** in the configuration category for it\.
   + To look for an option, turn on **Table View**, and then enter search terms into the search box\. As you type, the list gets shorter and shows only options that match your search terms\.

     When you see the option you're looking for, choose **Edit** in the configuration category that contains it\.  
![\[Table view of the configuration overview page of the Elastic Beanstalk console, showing an option search\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.table.search1.png)

1. Change settings, and then choose **Save**\.

1. Repeat the previous two steps in additional configuration categories, as needed\.

1. Choose **Apply**\.

Changes made to configuration options in the environment management console are applied directly to the environment\. These changes override settings for the same options in configuration files or saved configurations\. For details, see [Precedence](command-options.md#configuration-options-precedence)\.

For details about changing configuration options on a running environment using the Elastic Beanstalk console, see the topics under [Configuring Elastic Beanstalk environments](customize-containers.md)\.

## The EB CLI<a name="configuration-options-after-ebcli"></a>

You can update configuration option settings with the EB CLI by deploying source code that contains configuration files, applying settings from a saved configuration, or modifying the environment configuration directly with the eb config command\.

**Topics**
+ [Using configuration files \(`.ebextensions`\)](#configuration-options-after-ebcli-ebextensions)
+ [Using a saved configuration](#configuration-options-after-ebcli-savedconfig)
+ [Using eb config](#configuration-options-after-ebcli-ebconfig)
+ [Using eb setenv](#configuration-options-after-ebcli-ebsetenv)

### Using configuration files \(`.ebextensions`\)<a name="configuration-options-after-ebcli-ebextensions"></a>

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

Deploy your source code with eb deploy\.

```
~/workspace/my-app$ eb deploy
```

### Using a saved configuration<a name="configuration-options-after-ebcli-savedconfig"></a>

You can use the eb config command to apply a saved configuration to a running environment\. Use the `--cfg` option with the name of the saved configuration to apply its settings to your environment\.

```
$ eb config --cfg v1
```

In this example, `v1` is the name of a [previously created and saved configuration file](environment-configuration-methods-before.md#configuration-options-before-savedconfig)\.

Settings applied to an environment with this command override settings that were applied during environment creation, and settings defined in configuration files in your application source bundle\.

### Using eb config<a name="configuration-options-after-ebcli-ebconfig"></a>

The EB CLI's eb config command lets you set and remove option settings directly on an environment by using a text editor\.

When you run eb config, the EB CLI shows settings applied to your environment from all sources, including configuration files, saved configurations, recommended values, options set directly on the environment, and API defaults\.

**Note**  
eb config does not show environment properties\. To set environment properties that you can read from within your application, use [eb setenv](#configuration-options-after-ebcli-ebsetenv)\.

The following example shows settings applied in the `aws:autoscaling:launchconfiguration` namespace\. These settings include:
+ Two recommended values, for `IamInstanceProfile` and `InstanceType`, applied by the EB CLI during environment creation\.
+ The option `EC2KeyName`, set directly on the environment during creation based on repository configuration\.
+ API default values for the other options\.

```
ApplicationName: tomcat
DateUpdated: 2015-09-30 22:51:07+00:00
EnvironmentName: tomcat
SolutionStackName: 64bit Amazon Linux 2015.03 v2.0.1 running Tomcat 8 Java 8
settings:
...
aws:autoscaling:launchconfiguration:
    BlockDeviceMappings: null
    EC2KeyName: my-key
    IamInstanceProfile: aws-elasticbeanstalk-ec2-role
    ImageId: ami-1f316660
    InstanceType: t2.micro
...
```

**To set or change configuration options with eb config**

1. Run eb config to view your environment's configuration\.

   ```
   ~/workspace/my-app/$ eb config
   ```

1. Change any of the setting values using the default text editor\.

   ```
   aws:autoscaling:launchconfiguration:
       BlockDeviceMappings: null
       EC2KeyName: my-key
       IamInstanceProfile: aws-elasticbeanstalk-ec2-role
       ImageId: ami-1f316660
       InstanceType: t2.medium
   ```

1. Save the temporary configuration file and exit\.

1. The EB CLI updates your environment configuration\.

Setting configuration options with eb config overrides settings from all other sources\.

You can also remove options from your environment with eb config\.<a name="configuration-options-remove-ebcli"></a>

**To remove configuration options \(EB CLI\)**

1. Run eb config to view your environment's configuration\.

   ```
   ~/workspace/my-app/$ eb config
   ```

1. Replace any value shown with the string `null`\. You can also delete the entire line containing the option that you want to remove\.

   ```
   aws:autoscaling:launchconfiguration:
       BlockDeviceMappings: null
       EC2KeyName: my-key
       IamInstanceProfile: aws-elasticbeanstalk-ec2-role
       ImageId: ami-1f316660
       InstanceType: null
   ```

1. Save the temporary configuration file and exit\.

1. The EB CLI updates your environment configuration\.

Removing options from your environment with eb config allows settings for the same options to surface from configuration files in your application source bundle\. See [Precedence](command-options.md#configuration-options-precedence) for details\.

### Using eb setenv<a name="configuration-options-after-ebcli-ebsetenv"></a>

To set environment properties with the EB CLI, use eb setenv\.

```
~/workspace/my-app/$ eb setenv ENVVAR=TEST
INFO: Environment update is starting.
INFO: Updating environment my-env's configuration settings.
INFO: Environment health has transitioned from Ok to Info. Command is executing on all instances.
INFO: Successfully deployed new configuration to environment.
```

This command sets environment properties in the [`aws:elasticbeanstalk:application:environment` namespace](command-options-general.md#command-options-general-elasticbeanstalkapplicationenvironment)\. Environment properties set with eb setenv are available to your application after a short update process\.

View environment properties set on your environment with eb printenv\.

```
~/workspace/my-app/$ eb printenv
 Environment Variables:
     ENVVAR = TEST
```

## The AWS CLI<a name="configuration-options-after-awscli"></a>

You can update configuration option settings with the AWS CLI by deploying a source bundle that contains configuration files, applying a remotely stored saved configuration, or modifying the environment directly with the `aws elasticbeanstalk update-environment` command\.

**Topics**
+ [Using configuration files \(`.ebextensions`\)](#configuration-options-after-awscli-ebextensions)
+ [Using a saved configuration](#configuration-options-after-awscli-savedconfig)
+ [Using command line options](#configuration-options-after-awscli-commandline)

### Using configuration files \(`.ebextensions`\)<a name="configuration-options-after-awscli-ebextensions"></a>

To apply configuration files to a running environment with the AWS CLI, include them in the application source bundle that you upload to Amazon S3\.

For details about configuration files, see [\.Ebextensions](ebextensions.md)\.

```
~/workspace/my-app-v1.zip
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- index.php
`-- styles.css
```

**To upload an application source bundle and apply it to a running environment \(AWS CLI\)**

1. If you don't already have an Elastic Beanstalk bucket in Amazon S3, create one with `create-storage-location`:

   ```
   $ aws elasticbeanstalk create-storage-location
   {
       "S3Bucket": "elasticbeanstalk-us-west-2-123456789012"
   }
   ```

1. Upload your application source bundle to Amazon S3\.

   ```
   $ aws s3 cp sourcebundlev2.zip s3://elasticbeanstalk-us-west-2-123456789012/my-app/sourcebundlev2.zip
   ```

1. Create the application version\.

   ```
   $ aws elasticbeanstalk create-application-version --application-name my-app --version-label v2 --description MyAppv2 --source-bundle S3Bucket="elasticbeanstalk-us-west-2-123456789012",S3Key="my-app/sourcebundlev2.zip"
   ```

1. Update the environment\.

   ```
   $ aws elasticbeanstalk update-environment --environment-name my-env --version-label v2
   ```

### Using a saved configuration<a name="configuration-options-after-awscli-savedconfig"></a>

You can apply a saved configuration to a running environment with the `--template-name` option on the `aws elasticbeanstalk update-environment` command\.

The saved configuration must be in your Elastic Beanstalk bucket in a path named after your application under `resources/templates`\. For example, the `v1` template for the `my-app` application in the US West \(Oregon\) Region \(us\-west\-2\) for account 123456789012 is located at `s3://elasticbeanstalk-us-west-2-123456789012/resources/templates/my-app/v1`

**To apply a saved configuration to a running environment \(AWS CLI\)**
+ Specify the saved configuration in an `update-environment` call with the `--template-name` option\.

  ```
  $ aws elasticbeanstalk update-environment --environment-name my-env --template-name v1
  ```

Elastic Beanstalk places saved configurations in this location when you create them with `aws elasticbeanstalk create-configuration-template`\. You can also modify saved configurations locally and place them in this location yourself\.

### Using command line options<a name="configuration-options-after-awscli-commandline"></a>

**To change configuration options with a JSON document \(AWS CLI\)**

1. Define your option settings in JSON format in a local file\.

1. Run `update-environment` with the `--option-settings` option\.

   ```
   $ aws elasticbeanstalk update-environment --environment-name my-env --option-settings file://~/ebconfigs/as-zero.json
   ```

In this example, `as-zero.json` defines options that configure the environment with a minimum and maximum of zero instances\. This stops the instances in the environment without terminating the environment\.

**`~/ebconfigs/as-zero.json`**

```
[
    {
        "Namespace": "aws:autoscaling:asg",
        "OptionName": "MinSize",
        "Value": "0"
    },
    {
        "Namespace": "aws:autoscaling:asg",
        "OptionName": "MaxSize",
        "Value": "0"
    },
    {
        "Namespace": "aws:autoscaling:updatepolicy:rollingupdate",
        "OptionName": "RollingUpdateEnabled",
        "Value": "false"
    }
]
```

**Note**  
Setting configuration options with `update-environment` overrides settings from all other sources\.

You can also remove options from your environment with `update-environment`\.<a name="configuration-options-remove-awscli"></a>

**To remove configuration options \(AWS CLI\)**
+ Run the `update-environment` command with the `--settings-to-remove` option\.

  ```
  $ aws elasticbeanstalk update-environment --environment-name my-env --options-to-remove Namespace=aws:autoscaling:launchconfiguration,OptionName=InstanceType
  ```

Removing options from your environment with `update-environment` allows settings for the same options to surface from configuration files in your application source bundle\. If an option isn't configured using any of these methods, the API default value applies, if one exists\. See [Precedence](command-options.md#configuration-options-precedence) for details\.