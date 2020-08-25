# Elastic Beanstalk custom platforms<a name="custom-platforms"></a>

AWS Elastic Beanstalk supports custom platforms\. A custom platform is a more advanced customization than a [custom image](using-features.customenv.md) in several ways\. A custom platform lets you develop an entire new platform from scratch, customizing the operating system, additional software, and scripts that Elastic Beanstalk runs on platform instances\. This flexibility enables you to build a platform for an application that uses a language or other infrastructure software, for which Elastic Beanstalk doesn't provide a managed platform\. Compare that to custom images, where you modify an Amazon Machine Image \(AMI\) for use with an existing Elastic Beanstalk platform, and Elastic Beanstalk still provides the platform scripts and controls the platform's software stack\. In addition, with custom platforms you use an automated, scripted way to create and maintain your customization, whereas with custom images you make the changes manually over a running instance\.

**Note**  
Elastic Beanstalk custom platforms only support building an AMI from Amazon Linux AMI, RHEL 7, RHEL 6, or Ubuntu 16\.04 base AMIs\. Amazon Linux 2 or other operating systems aren't supported with the custom platforms feature\. 

To create a custom platform, you build an AMI from one of the supported operating systems—Ubuntu, RHEL, or Amazon Linux \(see the `flavor` entry in [Platform\.yaml file format](platform-yaml-format.md) for the exact version numbers\)—and add further customizations\. You create your own Elastic Beanstalk platform using [Packer](https://www.packer.io/), which is an open\-source tool for creating machine images for many platforms, including AMIs for use with Amazon Elastic Compute Cloud \(Amazon EC2\)\. An Elastic Beanstalk platform comprises an AMI configured to run a set of software that supports an application, and metadata that can include custom configuration options and default configuration option settings\.

Elastic Beanstalk manages Packer as a separate built\-in platform, and you don't need to worry about Packer configuration and versions\.

You create a platform by providing Elastic Beanstalk with a Packer template, and the scripts and files that the template invokes to build an AMI\. These components are packaged with a [platform definition file](#custom-platform-creating), which specifies the template and metadata, into a ZIP archive, known as a [platform definition archive](custom-platforms-pda.md)\.

When you create a custom platform, you launch a single instance environment without an Elastic IP that runs Packer\. Packer then launches another instance to build an image\. You can reuse this environment for multiple platforms and multiple versions of each platform\.

**Note**  
Custom platforms are AWS Region specific\. If you use Elastic Beanstalk in multiple Regions, you must create your platforms separately in each Region\.  
In certain circumstances, instances launched by Packer are not cleaned up and have to be manually terminated\. To learn how to manually clean up these instances, see [Packer instance cleanup](custom-platforms-packercleanup.md)\.

Users in your account can use your custom platforms by specifying a [platform ARN](AWSHowTo.iam.policies.arn.md) during environment creation\. These ARNs are returned by the eb platform create command that you used to create the custom platform\.

Each time you build your custom platform, Elastic Beanstalk creates a new platform version\. Users can specify a platform by name to get only the latest version of the platform, or include a version number to get a specific version\.

For example, to deploy the latest version of the custom platform with the ARN **MyCustomPlatformARN**, which could be version 3\.0, your EB CLI command line would look like this:

```
eb create -p MyCustomPlatformARN
```

To deploy version 2\.1 your EB CLI command line would look like this:

```
eb create -p MyCustomPlatformARN --version 2.1
```

You can apply tags to a custom platform version when you create it, and edit tags of existing custom platform versions\. For details, see [Tagging custom platform versions](custom-platforms-tagging.md)\.

## Creating a custom platform<a name="custom-platform-creating"></a>

To create a custom platform, the root of your application must include a platform definition file, `platform.yaml`, which defines the type of builder used to create the custom platform\. The format of this file is described in [Platform\.yaml file format](platform-yaml-format.md)\. You can create your custom platform from scratch, or use one of the [sample custom platforms](#custom-platforms-sample) as a starting point\.

## Using a sample custom platform<a name="custom-platforms-sample"></a>

One alternative to creating your own custom platform is to use one of the platform definition archive samples to bootstrap your custom platform\. The only items you have to configure in the samples before you can use them are a source AMI and a Region\.

**Note**  
Do not use an unmodified sample custom platform in production\. The goal of the samples is to show some of the functionality available for a custom platform, but they have not been hardened for production use\.

[NodePlatform\_Ubuntu\.zip](https://github.com/awslabs/eb-custom-platforms-samples/releases/download/v1.0.4/NodePlatform_Ubuntu.zip)  
This custom platform is based on **Ubuntu 16\.04** and supports **Node\.js 4\.4\.4**\. We use this custom platform for the examples in this section\.

[NodePlatform\_RHEL\.zip](https://github.com/awslabs/eb-custom-platforms-samples/releases/download/v1.0.4/NodePlatform_RHEL.zip)  
This custom platform is based on **RHEL 7\.2** and supports **Node\.js 4\.4\.4**\.

[NodePlatform\_AmazonLinux\.zip](https://github.com/awslabs/eb-custom-platforms-samples/releases/download/v1.0.4/NodePlatform_AmazonLinux.zip)  
This custom platform is based on **Amazon Linux 2016\.09\.1** and supports **Node\.js 4\.4\.4**\.

[TomcatPlatform\_Ubuntu\.zip](https://github.com/awslabs/eb-custom-platforms-samples/releases/download/v1.0.4/TomcatPlatform_Ubuntu.zip)  
This custom platform is based on **Ubuntu 16\.04** and supports **Tomcat 7/Java 8**\.

[CustomPlatform\_NodeSampleApp\.zip](https://github.com/awslabs/eb-custom-platforms-samples/releases/download/v1.0.4/CustomPlatform_NodeSampleApp.zip)  
A Node\.js sample that uses **express** and **ejs** to display a static webpage\.

[CustomPlatform\_TomcatSampleApp\.zip](https://github.com/awslabs/eb-custom-platforms-samples/releases/download/v1.0.4/CustomPlatform_TomcatSampleApp.zip)  
A Tomcat sample that displays a static webpage when deployed\.

Download the sample platform definition archive: `NodePlatform_Ubuntu.zip`\. This file contains a platform definition file, Packer template, scripts that Packer runs during image creation, and scripts and configuration files that Packer copies onto the builder instance during platform creation\.

**Example NodePlatform\_Ubuntu\.zip**  

```
|-- builder                 Contains files used by Packer to create the custom platform
|-- custom_platform.json    Packer template
|-- platform.yaml           Platform definition file
|-- ReadMe.txt              Briefly describes the sample
```

The platform definition file, `platform.yaml`, tells Elastic Beanstalk the name of the Packer template, `custom_platform.json`\.

```
version: "1.0"

provisioner:
  type: packer
  template: custom_platform.json
  flavor: ubuntu1604
```

The Packer template tells Packer how to build the AMIs for the platform, using an [Ubuntu AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) as a base for the platform image for HVM instance types\. The `provisioners` section tells Packer to copy all files in the `builder` folder within the archive to the instance, and to run the `builder.sh` script on the instance\. When the scripts complete, Packer creates an image from the modified instance\.

Elastic Beanstalk creates three environment variables that can be used to tag AMIs in Packer:

AWS\_EB\_PLATFORM\_ARN  
The ARN of the custom platform\.

AWS\_EB\_PLATFORM\_NAME  
The name of the custom platform\.

AWS\_EB\_PLATFORM\_VERSION  
The version of the custom platform\.

The sample `custom_platform.json` file uses these variables to define the following values that it uses in the scripts:
+ `platform_name`, which is set by `platform.yaml`
+ `platform_version`, which is set by `platform.yaml`
+ `platform_arn`, which is set by the main build script, `builder.sh`, which is shown at the end of the sample `custom_platform.json` file\.

The `custom_platform.json` file contains two properties that you have to provide values for: `source_ami` and `region`\. For details about choosing the right AMI and Region values, see [Updating Packer template](https://github.com/aws-samples/eb-custom-platforms-samples#updating-packer-template) in the *eb\-custom\-platforms\-samples* GitHub repository\.

**Example custom\_platform\.json**  

```
{
  "variables": {
    "platform_name": "{{env `AWS_EB_PLATFORM_NAME`}}",
    "platform_version": "{{env `AWS_EB_PLATFORM_VERSION`}}",
    "platform_arn": "{{env `AWS_EB_PLATFORM_ARN`}}"
  },
  "builders": [
    {
      ...
      "region": "",
      "source_ami": "",
      ...
    }
  ],
  "provisioners": [
    {...},
    {
      "type": "shell",
      "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo {{ .Path }}",
      "scripts": [
        "builder/builder.sh"
      ]
    }
  ]
}
```

The scripts and other files that you include in your platform definition archive will vary greatly depending on the modifications that you want to make to the instance\. The sample platform includes the following scripts:
+ `00-sync-apt.sh` – Commented out: `apt -y update`\. We commented out the command because it prompts the user for input, which breaks the automated package update\. This might be an Ubuntu issue\. However, running `apt -y update` is still recommended as a best practice\. For this reason, we left the command in the sample script for reference\.
+ `01-install-nginx.sh` – Installs nginx\.
+ `02-setup-platform.sh` – Installs `wget`, `tree`, and `git`\. Copies hooks and [logging configurations](using-features.logging.md) to the instance, and creates the following directories:
  + `/etc/SampleNodePlatform` – Where the container configuration file is uploaded during deployment\.
  + `/opt/elasticbeanstalk/deploy/appsource/` – Where the `00-unzip.sh` script uploads application source code during deployment \(see the [Platform script tools](custom-platforms-scripts.md) section for information about this script\)\.
  + `/var/app/staging/` – Where application source code is processed during deployment\.
  + `/var/app/current/` – Where application source code runs after processing\.
  + `/var/log/nginx/healthd/` – Where the [enhanced health agent](health-enhanced.md#health-enhanced-agent) writes logs\.
  + `/var/nodejs` – Where the Node\.js files are uploaded during deployment\.

Use the EB CLI to create your first custom platform with the sample platform definition archive\.

**To create a custom platform**

1. [Install the EB CLI](eb-cli3-install.md)\.

1. Create a directory in which you will extract the sample custom platform\.

   ```
   ~$ mkdir ~/custom-platform
   ```

1. Extract `NodePlatform_Ubuntu.zip` to the directory, and then change to the extracted directory\.

   ```
   ~$ cd ~/custom-platform
   ~/custom-platform$ unzip ~/NodePlatform_Ubuntu.zip
   ~/custom-platform$ cd NodePlatform_Ubuntu
   ```

1. Edit the `custom_platform.json` file, and provide values for the `source_ami` and `region` properties\. For details, see [Updating Packer template](https://github.com/aws-samples/eb-custom-platforms-samples#updating-packer-template)\.

1. Run [eb platform init](eb3-platform.md#eb3-platform-init) and follow the prompts to initialize a platform repository\.

   You can shorten eb platform to ebp\.
**Note**  
Windows PowerShell uses ebp as a command alias\. If you're running the EB CLI in Windows PowerShell, use the long form of this command: eb platform\.

   ```
   ~/custom-platform$ eb platform init
   ```

   This command also creates the directory `.elasticbeanstalk` in the current directory and adds the configuration file `config.yml` to the directory\. Don't change or delete this file, because Elastic Beanstalk relies on it when creating the custom platform\.

   By default, eb platform init uses the name of the current folder as the name of the custom platform, which would be `custom-platform` in this example\.

1. Run [eb platform create](eb3-platform.md#eb3-platform-create) to launch a Packer environment and get the ARN of the custom platform\. You'll need this value later when you create an environment from the custom platform\.

   ```
   ~/custom-platform$ eb platform create
   ...
   ```

   By default, Elastic Beanstalk creates the instance profile `aws-elasticbeanstalk-custom-platform-ec2-role` for custom platforms\. If, instead, you want to use an existing instance profile, add the option `-ip INSTANCE_PROFILE` to the [eb platform create](eb3-platform.md#eb3-platform-create) command\.
**Note**  
Packer will fail to create a custom platform if you use the Elastic Beanstalk default instance profile `aws-elasticbeanstalk-ec2-role`\.

   The EB CLI shows event output of the Packer environment until the build is complete\. You can exit the event view by pressing **Ctrl\+C**\. 

1. You can check the logs for errors using the [eb platform logs](eb3-platform.md#eb3-platform-logs) command\.

   ```
   ~/custom-platform$ eb platform logs
   ...
   ```

1. You can check on the process later with [eb platform events](eb3-platform.md#eb3-platform-events)\.

   ```
   ~/custom-platform$ eb platform events
   ...
   ```

1. Check the status of your platform with [eb platform status](eb3-platform.md#eb3-platform-status)\.

   ```
   ~/custom-platform$ eb platform status
   ...
   ```

When the operation completes, you have a platform that you can use to launch an Elastic Beanstalk environment\.

You can use the custom platform when creating an environment from the console\. See [The create new environment wizard](environments-create-wizard.md)\.

**To launch an environment on your custom platform**

1. Create a directory for your application\.

   ```
   ~$ mkdir custom-platform-app
   ~$ cd ~/custom-platform-app
   ```

1. Initialize an application repository\.

   ```
   ~/custom-platform-app$ eb init
   ...
   ```

1. Download the sample application [NodeSampleApp\.zip](samples/NodeSampleApp.zip)\.

1. Extract the sample application\.

   ```
   ~/custom-platform-app$ unzip ~/NodeSampleApp.zip
   ```

1. Run eb create \-p *CUSTOM\-PLATFORM\-ARN*, where *CUSTOM\-PLATFORM\-ARN* is the ARN returned by an eb platform create command, to launch an environment running your custom platform\.

   ```
   ~/custom-platform-app$ eb create -p CUSTOM-PLATFORM-ARN
   ...
   ```