# Setting up your PHP development environment<a name="php-development-environment"></a>

Set up a PHP development environment to test your application locally prior to deploying it to AWS Elastic Beanstalk\. This topic outlines development environment setup steps and links to installation pages for useful tools\.

For common setup steps and tools that apply to all languages, see [Configuring your development machine for use with Elastic Beanstalk](chapter-devenv.md)\.

**Topics**
+ [Installing PHP](#php-development-environment-php)
+ [Install Composer](#php-development-environment-libraries)
+ [Installing the AWS SDK for PHP](#php-development-environment-sdk)
+ [Installing an IDE or text editor](#php-development-environment-ide)

## Installing PHP<a name="php-development-environment-php"></a>

Install PHP and some common extensions\. If you don't have a preference, get the latest version\. Depending on your platform and available package manager, the steps will vary\.

On Amazon Linux, use yum:

```
$ sudo yum install php
$ sudo yum install php-mbstring
$ sudo yum install php-intl
```

**Note**  
To get specific PHP package versions that match the version on your Elastic Beanstalk [PHP platform version](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP), use the command `yum search php` to find available package versions, such as `php72`, `php72-mbstring`, and `php72-intl`\. Then use `sudo yum install package` to install them\.

On Ubuntu, use apt:

```
$ sudo apt install php-all-dev
$ sudo apt install php-intl
$ sudo apt install php-mbstring
```

On OS\-X, use brew:

```
$ brew install php
$ brew install php-intl
```

**Note**  
To get specific PHP package versions that match the version on your Elastic Beanstalk [PHP platform version](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP), see [Homebrew Formulae](https://formulae.brew.sh/formula/) for available PHP versions, such as `php@7.2`\. Then use `brew install package` to install them\.  
Depending on the version, `php-intl` might be included in the main PHP package and not exist as a separate package\.

On Windows 10, [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get Ubuntu and install PHP with apt\. For earlier versions, visit the download page at [windows\.php\.net](http://windows.php.net/download/) to get PHP, and read [this page](http://php.net/manual/en/install.windows.legacy.index.php#install.windows.legacy.extensions) for information about extensions\.

After installing PHP, reopen your terminal and run `php --version` to ensure that the new version has been installed and is the default\.

## Install Composer<a name="php-development-environment-libraries"></a>

Composer is a dependency manager for PHP\. You can use it to install libraries, track your application's dependencies, and generate projects for popular PHP frameworks\.

Install composer with the PHP script from getcomposer\.org\.

```
$ curl -s https://getcomposer.org/installer | php
```

The installer generates a PHAR file in the current directory\. Move this file to a location in your environment PATH so that you can use it as an executable\.

```
$ mv composer.phar ~/.local/bin/composer
```

Install libraries with the `require` command\.

```
$ composer require twig/twig
```

Composer adds libraries that you install locally to your project's [`composer.json` file](php-configuration-composer.md)\. When you deploy your project code, Elastic Beanstalk uses Composer to install the libraries listed in this file on your environment's application instances\.

If you run into issues installing Composer, see the [composer documentation](https://getcomposer.org/)\.

## Installing the AWS SDK for PHP<a name="php-development-environment-sdk"></a>

If you need to manage AWS resources from within your application, install the AWS SDK for PHP\. For example, with the SDK for PHP, you can use Amazon DynamoDB \(DynamoDB\) to store user and session information without creating a relational database\.

Install the SDK for PHP with Composer\.

```
$ composer require aws/aws-sdk-php
```

Visit the [AWS SDK for PHP homepage](https://aws.amazon.com/sdk-for-php/) for more information and installation instructions\.

## Installing an IDE or text editor<a name="php-development-environment-ide"></a>

Integrated development environments \(IDEs\) provide a wide range of features that facilitate application development\. If you haven't used an IDE for PHP development, try Eclipse and PHPStorm and see which works best for you\.
+  [Install Eclipse](https://www.eclipse.org/downloads/) 
+  [Install PhpStorm](https://www.jetbrains.com/phpstorm/) 

**Note**  
An IDE might add files to your project folder that you might not want to commit to source control\. To prevent committing these files to source control, use `.gitignore` or your source control tool's equivalent\.

If you just want to begin coding and don't need all of the features of an IDE, consider [installing Sublime Text](http://www.sublimetext.com/)\.