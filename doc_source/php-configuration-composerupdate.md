# Updating Composer<a name="php-configuration-composerupdate"></a>

[PHP platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP) ship with the latest version of Composer available at the time of release\. To keep Composer, PHP, and other libraries up to date, [upgrade your environment](using-features.platform.upgrade.md) whenever a platform update is available\. 

Between platform updates, you can use a [configuration file](ebextensions.md) to update Composer on the instances in your environment\. You may need to update Composer if you see an error when you try to install packages with a Composer file, or if you are unable to use the latest platform version\.

**Example \.ebextensions/composer\.config**  

```
commands:
  01updateComposer:
    command: export COMPOSER_HOME=/root && /usr/bin/composer.phar self-update 1.4.1

option_settings:
  - namespace: aws:elasticbeanstalk:application:environment
    option_name: COMPOSER_HOME
    value: /root
```

This configuration file configures composer to update itself to version 1\.4\.1\. Check the Composer [releases page](https://github.com/composer/composer/releases) on GitHub to find the latest version\.

**Note**  
If you omit the version number from the `composer.phar self-update` command, Composer will update to the latest version available every time you deploy your source code, and when new instances are provisioned by Auto Scaling\. This could cause scaling operations and deployments to fail if a version of Composer is released that is incompatible with your application\.