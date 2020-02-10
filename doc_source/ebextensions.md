# Advanced environment customization with configuration files \(`.ebextensions`\)<a name="ebextensions"></a>

You can add AWS Elastic Beanstalk configuration files \(`.ebextensions`\) to your web application's source code to configure your environment and customize the AWS resources that it contains\. Configuration files are YAML\- or JSON\-formatted documents with a `.config` file extension that you place in a folder named `.ebextensions` and deploy in your application source bundle\.

**Example \.ebextensions/network\-load\-balancer\.config**  
This example makes a simple configuration change\. It modifies a configuration option to set the type of your environment's load balancer to Network Load Balancer\.  

```
option_settings:
  aws:elasticbeanstalk:environment:
    LoadBalancerType: network
```

We recommend using YAML for your configuration files, because it's more readable than JSON\. YAML supports comments, multi\-line commands, several alternatives for using quotes, and more\. However, you can make any configuration change in Elastic Beanstalk configuration files identically using either YAML or JSON\.

**Tip**  
When you are developing or testing new configuration files, launch a clean environment running the default application and deploy to that\. Poorly formatted configuration files will cause a new environment launch to fail unrecoverably\.

The `option_settings` section of a configuration file defines values for [configuration options](command-options.md)\. Configuration options let you configure your Elastic Beanstalk environment, the AWS resources in it, and the software that runs your application\. Configuration files are only one of several ways to set configuration options\.

The [`Resources` section](environment-resources.md) lets you further customize the resources in your application's environment, and define additional AWS resources beyond the functionality provided by configuration options\. You can add and configure any resources supported by AWS CloudFormation, which Elastic Beanstalk uses to create environments\.

The other sections of a configuration file \(`packages`, `sources`, `files`, `users`, `groups`, `commands`, `container_commands`, and `services`\) let you configure the EC2 instances that are launched in your environment\. Whenever a server is launched in your environment, Elastic Beanstalk runs the operations defined in these sections to prepare the operating system and storage system for your application\.

For examples of commonly used \.ebextensions, see the [Elastic Beanstalk Configuration Files Repository](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files)\.

**Requirements**
+ **Location** – Place all of your configuration files in a single folder, named `.ebextensions`, in the root of your source bundle\. Folders starting with a dot can be hidden by file browsers, so make sure that the folder is added when you create your source bundle\. See [Create an application source bundle](applications-sourcebundle.md) for instructions\.
+ **Naming** – Configuration files must have the `.config` file extension\.
+ **Formatting** – Configuration files must conform to YAML or JSON specifications\.

  When using YAML, always use spaces to indent keys at different nesting levels\. For more information about YAML, see [YAML Ain't Markup Language \(YAML™\) Version 1\.1](http://yaml.org/spec/current.html)\.
+ **Uniqueness** – Use each key only once in each configuration file\.
**Warning**  
If you use a key \(for example, `option_settings`\) twice in the same configuration file, one of the sections will be dropped\. Combine duplicate sections into a single section, or place them in separate configuration files\.

The process for deploying varies slightly depending on the client that you use to manage your environments\. See the following sections for details:
+ [Elastic Beanstalk console](environment-configuration-methods-during.md#configuration-options-during-console-ebextensions)
+ [EB CLI](environment-configuration-methods-during.md#configuration-options-during-ebcli-ebextensions)
+ [AWS CLI](environment-configuration-methods-during.md#configuration-options-during-awscli-ebextensions)

**Topics**
+ [Option settings](ebextensions-optionsettings.md)
+ [Customizing software on Linux servers](customize-containers-ec2.md)
+ [Customizing software on Windows servers](customize-containers-windows-ec2.md)
+ [Adding and customizing Elastic Beanstalk environment resources](environment-resources.md)