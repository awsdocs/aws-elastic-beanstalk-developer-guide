# Installing your application's dependencies<a name="php-configuration-composer"></a>

Your application might have dependencies on other PHP packages\. You can configure your application to install these dependencies on the environment's Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. Alternatively, you can include your application's dependencies in the source bundle and deploy them with the application\. The following section discuss both of these ways\.

## Use a Composer file to install dependencies on instances<a name="php-configuration-composer.oninstances"></a>

Use a `composer.json` file in the root of your project source to use composer to install packages that your application requires on your environment's Amazon EC2 instances\.

**Example composer\.json**  

```
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

When a `composer.json` file is present, Elastic Beanstalk runs `composer.phar install` to install dependencies\. You can add options to append to the command by setting the [`composer_options` option](create_deploy_PHP.container.md#php-namespaces) in the `aws:elasticbeanstalk:container:php:phpini` namespace\.

## Include dependencies in source bundle<a name="php-configuration-composer.inbundle"></a>

If your application has a large number of dependencies, installing them might take a long time\. This can increase deployment and scaling operations, because dependencies are installed on every new instance\.

To avoid the negative impact on deployment time, use Composer in your development environment to resolve dependencies and install them into the `vendor` folder\.

**To include dependencies in your application source bundle**

1. Run the following command:

   ```
   % composer install
   ```

1. Include the generated `vendor` folder in the root of your application source bundle\.

When Elastic Beanstalk finds a `vendor` folder on the instance, it ignores the `composer.json` file \(even if it exists\)\. Your application then uses dependencies from the `vendor` folder\.