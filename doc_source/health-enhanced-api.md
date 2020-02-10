# Using enhanced health reporting with the Elastic Beanstalk API<a name="health-enhanced-api"></a>

Because AWS Elastic Beanstalk enhanced health reporting has role and solution stack requirements, you must update scripts and code that you used prior to the release of enhanced health reporting before you can use it\. To maintain backward compatibility, enhanced health reporting is not enabled by default when you create an environment using the Elastic Beanstalk API\.

You configure enhanced health reporting by setting the service role, the instance profile, and Amazon CloudWatch configuration options for your environment\. You can do this in three ways: by setting the configuration options in the `.ebextensions` folder, with saved configurations, or by configuring them directly in the `create-environment` call's `option-settings` parameter\.

To use the API, SDKs, or AWS command line interface \(CLI\) to create an environment that supports enhanced health, you must:
+ Create a service role and instance profile with the appropriate [permissions](concepts-roles.md)
+ Create a new environment with a new [platform version](concepts.platforms.md)
+ Set the health system type, instance profile, and service role [configuration options](command-options.md)

Use the following configuration options in the `aws:elasticbeanstalk:healthreporting:system`, `aws:autoscaling:launchconfiguration`, and `aws:elasticbeanstalk:environment` namespaces to configure your environment for enhanced health reporting\. 

## Enhanced health configuration options<a name="health-enhanced-api-options"></a>

**SystemType**

Namespace: `aws:elasticbeanstalk:healthreporting:system`

To enable enhanced health reporting, set to **enhanced**\.

**IamInstanceProfile**

Namespace: `aws:autoscaling:launchconfiguration`

Set to the name of an instance profile configured for use with Elastic Beanstalk\.

**ServiceRole**

Namespace: `aws:elasticbeanstalk:environment`

Set to the name of a service role configured for use with Elastic Beanstalk\.

**ConfigDocument** \(optional\)

Namespace: `aws:elasticbeanstalk:healthreporting:system`

A JSON document that defines the and instance and environment metrics to publish to CloudWatch\. For example:

```
{
  "CloudWatchMetrics":
    {
    "Environment":
      {
      "ApplicationLatencyP99.9":60,
      "InstancesSevere":60
      }
    "Instance":
      {
      "ApplicationLatencyP85":60,
      "CPUUser": 60
      }
    }
  "Version":1
}
```

**Note**  
Config documents may require special formatting, such as escaping quotes, depending on how you provide them to Elastic Beanstalk\. See [Providing custom metric config documents](health-enhanced-cloudwatch.md#health-enhanced-cloudwatch-configdocument) for examples\.