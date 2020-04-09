# Platform\.yaml file format<a name="platform-yaml-format"></a>

The `platform.yaml` file has the following format\.

```
version: "version-number"

provisioner:
   type: provisioner-type
   template: provisioner-template
   flavor: provisioner-flavor
        
metadata:
   maintainer: metadata-maintainer
   description: metadata-description
   operating_system_name: metadata-operating_system_name
   operating_system_version: metadata-operating_system_version
   programming_language_name: metadata-programming_language_name
   programming_language_version: metadata-programming_language_version
   framework_name: metadata-framework_name
   framework_version: metadata-framework_version

option_definitions:
   - namespace: option-def-namespace
     option_name: option-def-option_name
     description: option-def-description
     default_value: option-def-default_value

option_settings:
   - namespace: "option-setting-namespace"
     option_name: "option-setting-option_name"
     value: "option-setting-value"
```

Replace the placeholders with these values:

*version\-number*  
Required\. The version of the YAML definition\. Must be **1\.0**\.

*provisioner\-type*  
Required\. The type of builder used to create the custom platform\. Must be **packer**\.

*provisioner\-template*  
Required\. The JSON file containing the settings for *provisioner\-type*\.

*provisioner\-flavor*  
Optional\. The base operating system used for the AMI\. One of the following:     
amazon \(default\)  
Amazon Linux\. If not specified, the latest version of Amazon Linux when the platform is created\.  
Amazon Linux 2 isn't a supported operating system flavor\.  
ubuntu1604  
Ubuntu 16\.04 LTS  
rhel7  
RHEL 7  
rhel6  
RHEL 6

*metadata\-maintainer*  
Optional\. Contact information for the person who owns the platform \(100 characters\)\.

*metadata\-description*  
Optional\. Description of the platform \(2,000 characters\)\.

*metadata\-operating\_system\_name*  
Optional\. Name of the platform's operating system \(50 characters\)\. This value is available when filtering the output for the [ListPlatformVersions](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListPlatformVersions.html) API\.

*metadata\-operating\_system\_version*  
Optional\. Version of the platform's operating system \(20 characters\)\.

*metadata\-programming\_language\_name*  
Optional\. Programming language supported by the platform \(50 characters\)

*metadata\-programming\_language\_version*  
Optional\. Version of the platform's language \(20 characters\)\.

*metadata\-framework\_name*  
Optional\. Name of the web framework used by the platform \(50 characters\)\.

*metadata\-framework\_version*  
Optional\. Version of the platform's web framework \(20 characters\)\.

*option\-def\-namespace*  
Optional\. A namespace under `aws:elasticbeanstalk:container:custom` \(100 characters\)\.

*option\-def\-option\_name*  
Optional\. The option's name \(100 characters\)\. You can define up to 50 custom configuration options that the platform provides to users\.

*option\-def\-description*  
Optional\. Description of the option \(1,024 characters\)\.

*option\-def\-default\_value*  
Optional\. Default value used when the user doesn't specify one\.  
The following example creates the option **NPM\_START**\.  

```
options_definitions:
 -  namespace: "aws:elasticbeanstalk:container:custom:application"
    option_name: "NPM_START"
    description: "Default application startup command"
    default_value: "node application.js"
```

*option\-setting\-namespace*  
Optional\. Namespace of the option\.

*option\-setting\-option\_name*  
Optional\. Name of the option\. You can specify up to 50 [options provided by Elastic Beanstalk](command-options-general.md)\.

*option\-setting\-value*  
Optional\. Value used when the user doesn't specify one\.  
The following example creates the option **TEST**\.  

```
option_settings:
 - namespace: "aws:elasticbeanstalk:application:environment"
   option_name: "TEST"
   value: "This is a test"
```