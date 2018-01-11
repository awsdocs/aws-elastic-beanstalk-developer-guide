# Composer File<a name="php-configuration-composer"></a>

Use a `composer.json` file in the root of your project source to use composer to install packages that your application requires\.

**Example composer\.json**  

```
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

When a `composer.json` file is present, Elastic Beanstalk runs `composer.phar install` to install dependencies\. You can add options to append to the command by setting the `composer_options` option in the `aws:elasticbeanstalk:container:php:phpini` namespace\.