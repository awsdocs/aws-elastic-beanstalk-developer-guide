# Updating Composer<a name="php-configuration-composerupdate"></a>

You may have to update Composer if you see an error when you try to install packages with a Composer file, or if you are unable to use the latest platform version\. Between platform updates, you can use a [configuration file](ebextensions.md) to update Composer on the instances in your environment\.

**Example \.ebextensions/composer\.config**  

```
commands:
  01updateComposer:
    command: export COMPOSER_HOME=/root && /usr/bin/composer.phar self-update 1.9.3

option_settings:
  - namespace: aws:elasticbeanstalk:application:environment
    option_name: COMPOSER_HOME
    value: /root
```

This configuration file configures composer to update itself to version 1\.9\.3\. Check the Composer [releases page](https://github.com/composer/composer/releases) on GitHub to find the latest version\.

For more information about the Elastic Beanstalk PHP Platforms, including the version of Composer, see [PHP platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP) in the document *AWS Elastic Beanstalk Platforms*\.

**Note**  
If you omit the version number from the `composer.phar self-update` command, Composer will update to the latest version available every time you deploy your source code, and when new instances are provisioned by Auto Scaling\. This could cause scaling operations and deployments to fail if a version of Composer is released that is incompatible with your application\.