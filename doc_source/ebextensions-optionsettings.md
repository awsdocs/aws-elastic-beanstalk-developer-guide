# Option settings<a name="ebextensions-optionsettings"></a>

You can use the `option_settings` key to modify the Elastic Beanstalk configuration and define variables that can be retrieved from your application using environment variables\. Some namespaces allow you to extend the number of parameters, and specify the parameter names\. For a list of namespaces and configuration options, see [Configuration options](command-options.md)\.

Option settings can also be applied directly to an environment during environment creation or an environment update\. Settings applied directly to the environment override the settings for the same options in configuration files\. If you remove settings from an environment's configuration, settings in configuration files will take effect\. See [Precedence](command-options.md#configuration-options-precedence) for details\.

## Syntax<a name="ebextensions-optionsettings-syntax"></a>

The standard syntax for option settings is an array of objects, each having a `namespace`, `option_name` and `value` key\.

```
option_settings:
  - namespace:  namespace
    option_name:  option name
    value:  option value
  - namespace:  namespace
    option_name:  option name
    value:  option value
```

The `namespace` key is optional\. If you do not specify a namespace, the default used is `aws:elasticbeanstalk:application:environment`:

```
option_settings:
  - option_name:  option name
    value:  option value
  - option_name:  option name
    value:  option value
```

Elastic Beanstalk also supports a shorthand syntax for option settings that lets you specify options as key\-value pairs underneath the namespace:

```
option_settings:
  namespace:
    option name: option value
    option name: option value
```

## Examples<a name="ebextensions-optionsettings-snippet"></a>

The following examples set a Tomcat platform\-specific option in the `aws:elasticbeanstalk:container:tomcat:jvmoptions` namespace and an environment property named `MYPARAMETER`\.

In standard YAML format:

**Example \.ebextensions/options\.config**  

```
option_settings:
  - namespace:  aws:elasticbeanstalk:container:tomcat:jvmoptions
    option_name:  Xmx
    value:  256m
  - option_name: MYPARAMETER
    value: parametervalue
```

In shorthand format:

**Example \.ebextensions/options\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:tomcat:jvmoptions:
    Xmx: 256m
  aws:elasticbeanstalk:application:environment:
    MYPARAMETER: parametervalue
```

In JSON:

**Example \.ebextensions/options\.config**  

```
{
  "option_settings": [
    {
      "namespace": "aws:elasticbeanstalk:container:tomcat:jvmoptions",
      "option_name": "Xmx",
      "value": "256m"
    },
    {
      "option_name": "MYPARAMETER",
      "value": "parametervalue"
    }
  ]
}
```