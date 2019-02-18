# Using the AWS Elastic Beanstalk Go Platform<a name="go-environment"></a>

You can use AWS Elastic Beanstalk to run, build, and configure Go\-based applications\. For simple Go applications, there are two ways to deploy your application:
+ Provide a source bundle with a source file at the root called `application.go` that contains the main package for your application\. Elastic Beanstalk builds the binary using the following command:

  ```
  go build -o bin/application application.go
  ```

  After the application is built, Elastic Beanstalk starts it on port 5000\.
+ Provide a source bundle with a binary file called `application`\. The binary file can be located either at the root of the source bundle or in the `bin/` directory of the source bundle\. If you place the `application` binary file in both locations, Elastic Beanstalk uses the file in the `bin/` directory\.

  Elastic Beanstalk launches this application on port 5000\.

For more complex Go applications, there are two ways to deploy your application:
+ Provide a source bundle that includes your application source files, along with a [Buildfile](go-buildfile.md) and a [Procfile](go-procfile.md)\. The Buildfile includes a command to build the application, and the Procfile includes instructions to run the application\.
+ Provide a source bundle that includes your application binary files, along with a Procfile\. The Procfile includes instructions to run the application\.

**Execution Order**  
When you include multiple types of configuration in your application source bundle, they are executed in the following order\. Each step does not begin until the previous step completes\.   
Step 1: `commands`, `files` and `packages` defined in [configuration files](ebextensions.md)
Step 2: `Buildfile` command
Step 3: `container_commands` in configuration files
Step 4: `Procfile` commands \(all commands are run simultaneously\)
For more information on using `commands`, `files`, `packages` and `container_commands` in configuration files, see [Customizing Software on Linux Servers](customize-containers-ec2.md)

## Configuring Your Go Environment<a name="go-options"></a>

The Elastic Beanstalk Go platform provides a few platform\-specific options in addition to the standard options that all platforms have\. These options let you configure the nginx proxy that runs in front of your application to serve static files\.

You can use the AWS Management Console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your Go environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

### Log Options<a name="go-options-logs"></a>

The Log Options section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Static Files<a name="go-options-staticfiles"></a>

To improve performance, you can configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. You can set the virtual path and directory mappings in the **Static Files** section of the **Modify software** configuration page\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving Static Files](environment-cfg-staticfiles.md)\.

### Environment Properties<a name="go-options-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

Inside the Go environment running in Elastic Beanstalk, environment variables are accessible using the `os.Getenv` function\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code:

```
endpoint := os.Getenv("API_ENDPOINT")
```

See [Environment Properties and Other Software Settings](environments-cfg-softwaresettings.md) for more information\.

## Go Configuration Namespaces<a name="go-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Go platform supports one platform\-specific configuration namespace in addition to the [namespaces supported by all platforms](command-options-general.md)\. The `aws:elasticbeanstalk:container:golang:staticfiles` namespace lets you define options that map paths on your web application to folders in your application source bundle that contain static content\.

For example, this [configuration file](ebextensions.md) tells the proxy server to serve files in the `myimages` folder at the path `/images`:

**Example \.ebextensions/go\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:golang:staticfiles:
    /html: statichtml
    /images: staticimages
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration Options](command-options.md) for more information\.