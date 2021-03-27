# Extending Elastic Beanstalk Linux platforms<a name="platforms-linux-extend"></a>

The [AWS Elastic Beanstalk Linux platforms](platforms-linux.md) provide a lot of functionality out of the box to support developing and running your application\. When necessary, you can extend the platforms in several ways to configure options, install software, add files and start\-up commands, provide build and runtime instructions, and add initialization scripts that run in various provisioning stages of your environment's Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.

## Buildfile and Procfile<a name="platforms-linux-extend.build-proc"></a>

Some platforms allow you to customize how you build or prepare your application, and to specify the processes that run your application\. Each individual platform topic specifically mentions *Buildfile* and/or *Procfile* if the platform supports them\. Look for your specific platform under [Elastic Beanstalk platforms](concepts-all-platforms.md)\.

For all supporting platforms, syntax and semantics are identical, and are as described on this page\. Individual platform topics mention specific usage of these files for building and running applications in their respective languages\.

### Buildfile<a name="platforms-linux-extend.build"></a>

To specify a custom build and configuration command for your application, place a file named `Buildfile` in the root directory of your application source\. The file name is case sensitive\. Use the following syntax for your `Buildfile`\.

```
<process_name>: <command>
```

The command in your `Buildfile` must match the following regular expression: `^[A-Za-z0-9_-]+:\s*[^\s].*$`

Elastic Beanstalk doesn't monitor the application that is run with a `Buildfile`\. Use a `Buildfile` for commands that run for short periods and terminate after completing their tasks\. For long\-running application processes that should not exit, use a [Procfile](#platforms-linux-extend.proc)\.

All paths in the `Buildfile` are relative to the root of the source bundle\. In the following example of a `Buildfile`, `build.sh` is a shell script located at the root of the source bundle\.

**Example Buildfile**  

```
make: ./build.sh
```

If you want to provide custom build steps, we recommend that you use `predeploy` platform hooks for anything but the simplest commands, instead of a `Buildfile`\. Platform hooks allow richer scripts and better error handling\. Platform hooks are described in the next section\.

### Procfile<a name="platforms-linux-extend.proc"></a>

To specify custom commands to start and run your application, place a file named `Procfile` in the root directory of your application source\. The file name is case sensitive\. Use the following syntax for your `Procfile`\. You can specify one or more commands\.

```
<process_name1>: <command1>
<process_name2>: <command2>
...
```

Each line in your `Procfile` must match the following regular expression: `^[A-Za-z0-9_-]+:\s*[^\s].*$`

Use a `Procfile` for long\-running application processes that shouldn't exit\. Elastic Beanstalk expects processes run from the `Procfile` to run continuously\. Elastic Beanstalk monitors these processes and restarts any process that terminates\. For short\-running processes, use a [Buildfile](#platforms-linux-extend.build)\.

All paths in the `Procfile` are relative to the root of the source bundle\. The following example `Procfile` defines three processes\. The first one, called `web` in the example, is the *main web application*\.

**Example Procfile**  

```
web: bin/myserver
cache: bin/mycache
foo: bin/fooapp
```

Elastic Beanstalk configures the proxy server to forward requests to your main web application on port 8000, and you can configure this port number\. A common use for a `Procfile` is to pass this port number to your application as a command argument\. For details about proxy configuration, expand the *Reverse proxy configuration* section on this page\.

Elastic Beanstalk captures standard output and error streams from `Procfile` processes in log files\. Elastic Beanstalk names the log files after the process and stores them in `/var/log`\. For example, the `web` process in the preceding example generates logs named `web-1.log` and `web-1.error.log` for `stdout` and `stderr`, respectively\.

## Platform hooks<a name="platforms-linux-extend.hooks"></a>

Platform hooks are specifically designed to extend your environment's platform\. These are custom scripts and other executable files that you deploy as part of your application's source code, and Elastic Beanstalk runs during various instance provisioning stages\.

**Note**  
Platform hooks aren't supported on Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\.

### Application deployment platform hooks<a name="platforms-linux-extend.hooks.appdeploy"></a>

An *application deployment* occurs when you provide a new source bundle for deployment, or when you make a configuration change that requires termination and recreation of all environment instances\.

To provide platform hooks that run during an application deployment, place the files under the `.platform/hooks` directory in your source bundle, in one of the following subdirectories\.
+ `prebuild` – Files here run after the Elastic Beanstalk platform engine downloads and extracts the application source bundle, and before it sets up and configures the application and web server\.

  The `prebuild` files run after running commands found in the [commands](customize-containers-ec2.md#linux-commands) section of any configuration file and before running `Buildfile` commands\.
+ `predeploy` – Files here run after the Elastic Beanstalk platform engine sets up and configures the application and web server, and before it deploys them to their final runtime location\.

  The `predeploy` files run after running commands found in the [container\_commands](customize-containers-ec2.md#linux-container-commands) section of any configuration file and before running `Procfile` commands\.
+ `postdeploy` – Files here run after the Elastic Beanstalk platform engine deploys the application and proxy server\.

  This is the last deployment workflow step\.

### Configuration deployment platform hooks<a name="platforms-linux-extend.hooks.configdeploy"></a>

A *configuration deployment* occurs when you make configuration changes that only update environment instances without recreating them\. The following option updates cause a configuration update\.
+ [Environment properties and platform\-specific settings](environments-cfg-softwaresettings.md)
+ [Static files](environment-cfg-staticfiles.md)
+ [AWS X\-Ray daemon](environment-configuration-debugging.md)
+ [Log storage and streaming](environments-cfg-logging.md)
+ Application port \(for details, expand the *Reverse proxy configuration* section on this page\)

To provide hooks that run during a configuration deployment, place them under the `.platform/confighooks` directory in your source bundle\. The same three subdirectories as for application deployment hooks apply\.

### More about platform hooks<a name="platforms-linux-extend.hooks.more"></a>

Hook files can be binary files, or script files starting with a `#!` line containing their interpreter path, such as `#!/bin/bash`\. All files have to have execute permission\. Use `chmod +x` to set execute permission on your hook files\.

Elastic Beanstalk runs files in each one of these directories in lexicographical order of file names\. All files run as the `root` user\. The current working directory \(cwd\) for platform hooks is the application's root directory\. For `prebuild` and `predeploy` files it's the application staging directory, and for `postdeploy` files it's the current application directory\. If one of the files fails \(exits with a non\-zero exit code\), the deployment aborts and fails\.

Hook files have access to all environment properties that you've defined in application options, and to the system environment variables `HOME`, `PATH`, and `PORT`\. 

To get values of environment variables and other configuration options into your platform hook scripts, you can use the `get-config` utility that Elastic Beanstalk provides on environment instances\. For details, see [Platform script tools](custom-platforms-scripts.md)\.

## Configuration files<a name="platforms-linux-extend.config-files"></a>

You can add [configuration files](ebextensions.md) to the `.ebextensions` directory of your application's source code to configure various aspects of your Elastic Beanstalk environment\. Among other things, configuration files let you customize software and other files on your environment's instances and run initialization commands on the instances\. For more information, see [Customizing software on Linux servers](customize-containers-ec2.md)\.

You can also set [configuration options](command-options.md) using configuration files\. Many of the options control platform behavior, and some of these options are [platform specific](command-options-specific.md)\.

On Amazon Linux 2 platforms, we recommend using *Buildfile*\. *Procfile*, and *platform hooks* to configure and run custom code on your environment instances during instance provisioning\. These mechanisms are described in the previous sections on this page\. You can still use commands and container commands in `.ebextensions` configuration files, but they aren't as easy to work with\. For example, writing command scripts inside a YAML file can be challenging from a syntax standpoint\. You still need to use `.ebextensions` configuration files for any script that needs a reference to a AWS CloudFormation resource\.

## Reverse proxy configuration<a name="platforms-linux-extend.proxy"></a>

All Amazon Linux 2 platform versions use nginx as their default reverse proxy server\. The Tomcat, Node\.js, PHP, and Python platform also support Apache HTTPD as an alternative\. To select Apache on these platforms, set the `ProxyServer` option in the `aws:elasticbeanstalk:environment:proxy` namespace to `apache`\. All platforms enable proxy server configuration in a uniform way, as described in this section\.

**Note**  
On Amazon Linux AMI platform versions \(preceding Amazon Linux 2\) you might have to configure proxy servers differently\. You can find these legacy details under the [respective platform topics](concepts-all-platforms.md) in this guide\.

Elastic Beanstalk configures the proxy server on your environment's instances to forward web traffic to the main web application on the root URL of the environment; for example, `http://my-env.elasticbeanstalk.com`\.

By default, Elastic Beanstalk configures the proxy to forward requests coming in on port 80 to your main web application on port 5000\. You can configure this port number by setting the `PORT` environment property using the [aws:elasticbeanstalk:application:environment](command-options-general.md#command-options-general-elasticbeanstalkapplicationenvironment) namespace in a configuration file, as shown in the following example\.

```
option_settings:
  - namespace:  aws:elasticbeanstalk:application:environment
    option_name:  PORT
    value:  <main_port_number>
```

For more information about setting environment variables for your application, see [Option settings](ebextensions-optionsettings.md)\.

Your application should listen on the port that is configured for it in the proxy\. If you change the default port using the `PORT` environment property, your code can access it by reading the value of the `PORT` environment variable\. For example, call `os.Getenv("PORT")` in Go, or `System.getenv("PORT")` in Java\. If you configure your proxy to send traffic to multiple application processes, you can configure several environment properties, and use their values in both proxy configuration and your application code\. Another option is to pass the port value to the process as a command argument in the `Procfile`\. For details on that, expand the *Buildfile and Procfile* section on this page\.

### Configuring nginx<a name="platforms-linux-extend.proxy.nginx"></a>

Elastic Beanstalk uses nginx as the default reverse proxy to map your application to your Elastic Load Balancing load balancer\. Elastic Beanstalk provides a default nginx configuration that you can extend or override completely with your own configuration\.

**Note**  
When you add or edit an nginx `.conf` configuration file, be sure to encode it as UTF\-8\.

To extend the Elastic Beanstalk default nginx configuration, add `.conf` configuration files to a folder named `.platform/nginx/conf.d/` in your application source bundle\. The Elastic Beanstalk nginx configuration includes `.conf` files in this folder automatically\.

```
~/workspace/my-app/
|-- .platform
|   `-- nginx
|       `-- conf.d
|           `-- myconf.conf
`-- other source files
```

To override the Elastic Beanstalk default nginx configuration completely, include a configuration in your source bundle at `.platform/nginx/nginx.conf`:

```
~/workspace/my-app/
|-- .platform
|   `-- nginx
|       `-- nginx.conf
`-- other source files
```

If you override the Elastic Beanstalk nginx configuration, add the following line to your `nginx.conf` to pull in the Elastic Beanstalk configurations for [Enhanced health reporting and monitoring](health-enhanced.md), automatic application mappings, and static files\.

```
 include conf.d/elasticbeanstalk/*.conf;
```

### Configuring Apache HTTPD<a name="platforms-linux-extend.proxy.httpd"></a>

The Tomcat, Node\.js, PHP, and Python platforms allow you to choose the Apache HTTPD proxy server as an alternative to nginx\. This isn't the default\. The following example configures Elastic Beanstalk to use Apache HTTPD\.

**Example \.ebextensions/httpd\-proxy\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: apache
```
You can extend the Elastic Beanstalk default Apache configuration with your additional configuration files\. Alternatively, you can override the Elastic Beanstalk default Apache configuration completely\.  
To extend the Elastic Beanstalk default Apache configuration, add `.conf` configuration files to a folder named `.platform/httpd/conf.d` in your application source bundle\. The Elastic Beanstalk Apache configuration includes `.conf` files in this folder automatically\.  

```
~/workspace/my-app/
|-- .ebextensions
|   -- httpd-proxy.config
|-- .platform
|   -- httpd
|      -- conf.d
|         -- port5000.conf
|         -- ssl.conf
-- index.jsp
```
For example, the following Apache 2\.4 configuration adds a listener on port 5000\.  

**Example \.platform/httpd/conf\.d/port5000\.conf**  

```
listen 5000
<VirtualHost *:5000>
  <Proxy *>
    Require all granted
  </Proxy>
  ProxyPass / http://localhost:8080/ retry=0
  ProxyPassReverse / http://localhost:8080/
  ProxyPreserveHost on

  ErrorLog /var/log/httpd/elasticbeanstalk-error_log
</VirtualHost>
```
To override the Elastic Beanstalk default Apache configuration completely, include a configuration in your source bundle at `.platform/httpd/conf/httpd.conf`\.  

```
~/workspace/my-app/
|-- .ebextensions
|   -- httpd-proxy.config
|-- .platform
|   `-- httpd
|       `-- conf
|           `-- httpd.conf
`-- index.jsp
```
If you override the Elastic Beanstalk Apache configuration, add the following lines to your `httpd.conf` to pull in the Elastic Beanstalk configurations for [Enhanced health reporting and monitoring](health-enhanced.md), automatic application mappings, and static files\.  

```
IncludeOptional conf.d/elasticbeanstalk/*.conf
```

If you're migrating your Elastic Beanstalk application to an Amazon Linux 2 platform, be sure to also read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

**Topics**
+ [Application example with extensions](#platforms-linux-extend.example)
+ [Instance deployment workflow](#platforms-linux-extend.workflow)
+ [Platform script tools](custom-platforms-scripts.md)

## Application example with extensions<a name="platforms-linux-extend.example"></a>

The following example demonstrates an application source bundle with several extensibility features that Elastic Beanstalk Amazon Linux 2 platforms support: a `Procfile`, `.ebextensions` configuration files, custom hooks, and proxy configuration files\.

```
~/my-app/
|-- web.jar
|-- Procfile
|-- readme.md
|-- .ebextensions/
|   |-- options.config        # Option settings
|   `-- cloudwatch.config     # Other .ebextensions sections, for example files and container commands
`-- .platform/
    |-- nginx/                # Proxy configuration
    |   |-- nginx.conf
    |   `-- conf.d/
    |       `-- custom.conf
    |-- hooks/                # Application deployment hooks
    |   |-- prebuild/
    |   |   |-- 01_set_secrets.sh
    |   |   `-- 12_update_permissions.sh
    |   |-- predeploy/
    |   |   `-- 01_some_service_stop.sh
    |   `-- postdeploy/
    |       |-- 01_set_tmp_file_permissions.sh
    |       |-- 50_run_something_after_app_deployment.sh
    |       `-- 99_some_service_start.sh
    `-- confighooks/          # Configuration deployment hooks
        |-- prebuild/
        |   `-- 01_set_secrets.sh
        |-- predeploy/
        |   `-- 01_some_service_stop.sh
        `-- postdeploy/
            |-- 01_run_something_after_config_deployment.sh
            `-- 99_some_service_start.sh
```

**Note**  
Some of these extensions aren't supported on Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\.

## Instance deployment workflow<a name="platforms-linux-extend.workflow"></a>

With many ways to extend your environment's platform, it's useful to know what happens whenever Elastic Beanstalk provisions an instance or runs a deployment to an instance\. The following diagram shows this entire deployment workflow\. It depicts the different phases in a deployment, and the steps that Elastic Beanstalk takes in each phase\.

**Notes**  
The diagram doesn't represent the complete set of steps that Elastic Beanstalk takes on environment instances during deployment\. We provide this diagram for illustration, to provide you with the order and context for the execution of your customizations\.
For simplicity, the diagram mentions only the `.platform/hooks/*` hook subdirectories \(for application deployments\), and not the `.platform/confighooks/*` hook subdirectories \(for configuration deployments\)\. Hooks in the latter subdirectories run during exactly the same steps as hooks in corresponding subdirectories shown in the diagram\.

![\[Order of running extensions on an instance in an environment using an Amazon Linux 2 platform version\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/platforms-linux-extend-order.png)

The following list details the deployment phases and steps\.

1. **Initial steps**

   Elastic Beanstalk downloads and extracts your application\. After each one of these steps, Elastic Beanstalk runs one of the extensibility steps\.

   1. Runs commands found in the [commands:](customize-containers-ec2.md#linux-commands) section of any configuration file\.

   1. Runs any executable files found in the `.platform/hooks/prebuild` directory of your source bundle \(`.platform/confighooks/prebuild` for a configuration deployment\)\.

1. **Configure**

   Elastic Beanstalk configures your application and the proxy server\.

   1. Runs the commands found in the `Buildfile` in your source bundle\.

   1. Copies your custom proxy configuration files, if you have any in the `.platform/nginx` directory of your source bundle, to their runtime location\.

   1. Runs commands found in the [container\_commands:](customize-containers-ec2.md#linux-container-commands) section of any configuration file\.

   1. Runs any executable files found in the `.platform/hooks/predeploy` directory of your source bundle \(`.platform/confighooks/predeploy` for a configuration deployment\)\.

1. **Deploy**

   Elastic Beanstalk deploys and runs your application and the proxy server\.

   1. Runs the command found in the `Procfile` file in your source bundle\.

   1. Runs or reruns the proxy server with your custom proxy configuration files, if you have any\.

   1. Runs any executable files found in the `.platform/hooks/postdeploy` directory of your source bundle \(`.platform/confighooks/postdeploy` for a configuration deployment\)\.
