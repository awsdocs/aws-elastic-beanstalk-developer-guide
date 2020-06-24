# Using the Elastic Beanstalk Go platform<a name="go-environment"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

You can use AWS Elastic Beanstalk to run, build, and configure Go\-based applications\. For simple Go applications, there are two ways to deploy your application:
+ Provide a source bundle with a source file at the root called `application.go` that contains the main package for your application\. Elastic Beanstalk builds the binary using the following command:

  ```
  go build -o bin/application application.go
  ```

  After the application is built, Elastic Beanstalk starts it on port 5000\.
+ Provide a source bundle with a binary file called `application`\. The binary file can be located either at the root of the source bundle or in the `bin/` directory of the source bundle\. If you place the `application` binary file in both locations, Elastic Beanstalk uses the file in the `bin/` directory\.

  Elastic Beanstalk launches this application on port 5000\.

In both cases, with Go 1\.11 or later, you can also provide module requirements in a file called `go.mod`\. For more information, see [Migrating to Go Modules](https://blog.golang.org/migrating-to-go-modules) in the Go blog\.

For more complex Go applications, there are two ways to deploy your application:
+ Provide a source bundle that includes your application source files, along with a [Buildfile](go-buildfile.md) and a [Procfile](go-procfile.md)\. The Buildfile includes a command to build the application, and the Procfile includes instructions to run the application\.
+ Provide a source bundle that includes your application binary files, along with a Procfile\. The Procfile includes instructions to run the application\.

The Go platform includes a proxy server to serve static assets and forward traffic to your application\. You can [extend or override the default proxy configuration](go-nginx.md) for advanced scenarios\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring your Go environment<a name="go-options"></a>

The Go platform settings let you fine\-tune the behavior of your Amazon EC2 instances\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the Elastic Beanstalk console\.

Use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your Go environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Log options<a name="go-options-logs"></a>

The Log Options section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Static files<a name="go-options-staticfiles"></a>

To improve performance, the **Static files** section lets you configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. For each directory, you set the virtual path to directory mapping\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

### Environment properties<a name="go-options-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

Inside the Go environment running in Elastic Beanstalk, environment variables are accessible using the `os.Getenv` function\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code:

```
endpoint := os.Getenv("API_ENDPOINT")
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## Go configuration namespace<a name="go-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Go platform doesn't define any platform\-specific namespaces\. You can configure the proxy to serve static files by using the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace\. For details and an example, see [Serving static files](environment-cfg-staticfiles.md)\.

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## The Amazon Linux AMI \(preceding Amazon Linux 2\) Go platform<a name="go.alami"></a>

If your Elastic Beanstalk Go environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Go configuration namespaces<a name="go.alami.namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Amazon Linux AMI Go platform supports one platform\-specific configuration namespace in addition to the [namespaces supported by all platforms](command-options-general.md)\. The `aws:elasticbeanstalk:container:golang:staticfiles` namespace lets you define options that map paths on your web application to folders in your application source bundle that contain static content\.

For example, this [configuration file](ebextensions.md) tells the proxy server to serve files in the `staticimages` folder at the path `/images`:

**Example \.ebextensions/go\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:golang:staticfiles:
    /html: statichtml
    /images: staticimages
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.