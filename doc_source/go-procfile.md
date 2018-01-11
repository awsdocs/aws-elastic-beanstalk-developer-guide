# Configuring the Application Process with a Procfile<a name="go-procfile"></a>

To specify custom commands to start a Go application, include a file called `Procfile` at the root of your source bundle\. The file name is case sensitive\. Use the following format for the `Procfile`: 

```
<process_name>: <command>
```

Each line in your Procfile must conform to the following regular expression: `^[A-Za-z0-9_]+:\s*.+$`\.

Elastic Beanstalk expects processes run from the `Procfile` to run continuously\. Elastic Beanstalk monitors these applications and restarts any process that terminates\. For short\-running processes, use a Buildfile command\.

You can use any name for your Go application, as long as it conforms to the aforementioned regular expression\. You must call the main application `web`\.

```
web: bin/server
queue_process: bin/queue_processor
foo: bin/fooapp
```

Elastic Beanstalk exposes the main `web` application on the root URL of the environment; for example, `http://my-go-env.elasticbeanstalk.com`\.

Elastic Beanstalk configures the nginx proxy to forward requests to your application on the port number specified in the `PORT` environment variable for your application\. Your application should always listen on that port\. You can access this variable within your application by calling the `os.Getenv("PORT")` method\.

Elastic Beanstalk uses the port number specified in the `PORT` option setting for the port for the first application in the `Procfile`, and then increments the port number for each subsequent application in the `Procfile` by 100\. If the `PORT` option is not set, Elastic Beanstalk uses 5000 for the initial port\.

In the preceding example, the `PORT` environment variable for the `web` application is 5000, the `queue_process` application is 5100, and the `foo` application is 5200\. 

You can specify the initial port by setting the `PORT` option with the aws:elasticbeanstalk:application:environment namespace, as shown in the following example\. 

```
option_settings:
  - namespace:  aws:elasticbeanstalk:application:environment
    option_name:  PORT
    value:  <first_port_number>
```

For more information about setting environment variables for your application, see [Option Settings](ebextensions-optionsettings.md)\.

Elastic Beanstalk also runs any application whose name does not have the `web_` prefix, but these applications are not available from outside of your instance\.

Standard output and error streams from processes started with a `Procfile` are captured in log files named after the process and stored in `/var/log`\. For example, the `web` process in the preceding example generates logs named `web-1.log` and `web-1.error.log` for `stdout` and `stderr`, respectively\.

All paths in the `Procfile` are relative to the root of the source bundle\. If you know in advance where the files reside on the instance, you can include absolute paths in the `Procfile`\. 