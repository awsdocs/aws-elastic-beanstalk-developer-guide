# Configuring the application process with a Procfile<a name="go-procfile"></a>

To specify custom commands to start a Go application, include a file called `Procfile` at the root of your source bundle\.

For details about writing and using a `Procfile`, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

**Example Procfile**  

```
web: bin/server
queue_process: bin/queue_processor
foo: bin/fooapp
```

You must call the main application `web`, and list it as the first command in your `Procfile`\. Elastic Beanstalk exposes the main `web` application on the root URL of the environment; for example, `http://my-go-env.elasticbeanstalk.com`\.

Elastic Beanstalk also runs any application whose name does not have the `web_` prefix, but these applications are not available from outside of your instance\.

Elastic Beanstalk expects processes run from the `Procfile` to run continuously\. Elastic Beanstalk monitors these applications and restarts any process that terminates\. For short\-running processes, use a [Buildfile](go-buildfile.md) command\.

## Using a Procfile on Amazon Linux AMI \(preceding Amazon Linux 2\)<a name="go-procfile.alami"></a>

If your Elastic Beanstalk Go environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Port passing<a name="go-procfile.alami.ports"></a>

Elastic Beanstalk configures the nginx proxy to forward requests to your application on the port number specified in the `PORT` [environment property](go-environment.md#go-options) for your application\. Your application should always listen on that port\. You can access this variable within your application by calling the `os.Getenv("PORT")` method\.

Elastic Beanstalk uses the port number specified in the `PORT` environment property for the port for the first application in the `Procfile`, and then increments the port number for each subsequent application in the `Procfile` by 100\. If the `PORT` environment property is not set, Elastic Beanstalk uses 5000 for the initial port\.

In the preceding example, the `PORT` environment property for the `web` application is 5000, the `queue_process` application is 5100, and the `foo` application is 5200\. 

You can specify the initial port by setting the `PORT` option with the [aws:elasticbeanstalk:application:environment](command-options-general.md#command-options-general-elasticbeanstalkapplicationenvironment) namespace, as shown in the following example\. 

```
option_settings:
  - namespace:  aws:elasticbeanstalk:application:environment
    option_name:  PORT
    value:  <first_port_number>
```

For more information about setting environment properties for your application, see [Option settings](ebextensions-optionsettings.md)\.