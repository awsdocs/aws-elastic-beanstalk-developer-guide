# Custom options<a name="configuration-options-custom"></a>

Use the `aws:elasticbeanstalk:customoption` namespace to define options and values that can be read in `Resources` blocks in other configuration files\. Use custom options to collect user specified settings in a single configuration file\.

For example, you may have a complex configuration file that defines a resource that can be configured by the user launching the environment\. If you use `Fn::GetOptionSetting` to retrieve the value of a custom option, you can put the definition of that option in a different configuration file, where it is more easily discovered and modified by the user\.

Also, because they are configuration options, custom options can be set at the API level to override values set in a configuration file\. See [Precedence](command-options.md#configuration-options-precedence) for more information\.

Custom options are defined like any other option:

```
option_settings:
  aws:elasticbeanstalk:customoption:
    option name: option value
```

For example, the following configuration file creates an option named `ELBAlarmEmail` and sets the value to `someone@example.com`:

```
option_settings:
  aws:elasticbeanstalk:customoption:
    ELBAlarmEmail: someone@example.com
```

Elsewhere, a configuration file defines an SNS topic that reads the option with `Fn::GetOptionSetting` to populate the value of the `Endpoint` attribute:

```
Resources:
  MySNSTopic:
    Type: AWS::SNS::Topic
    Properties:
      Subscription:
        - Endpoint: 
            Fn::GetOptionSetting:
              OptionName: ELBAlarmEmail
              DefaultValue: nobody@example.com
          Protocol: email
```

You can find more example snippets using `Fn::GetOptionSetting` at [Adding and customizing Elastic Beanstalk environment resources](environment-resources.md)\.