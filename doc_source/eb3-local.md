# eb local<a name="eb3-local"></a>

## Description<a name="eb3-localdescription"></a>

Use eb local run to run your application's containers locally in Docker\. Check the application's container status with eb local status\. Open the application in a web browser with eb local open\. Retrieve the location of the application's logs with eb local logs\.

eb local setenv and eb local printenv let you set and view environment variables that are provided to the Docker containers that you run locally with eb local run\.

You must run all eb local commands in the project directory of a Docker application that has been initialized as an EB CLI repository by using eb init\.

**Note**  
Use eb local on a local computer running Linux or macOS\. The command doesn't support Windows\.  
Before using the command on macOS, install Docker for Mac, and ensure that boot2docker isn't installed \(or isn't in the execution path\)\. The eb local command tries to use boot2docker if it's present, but doesn't work well with it on macOS\.

## Syntax<a name="eb3-localsyntax"></a>

eb local run

eb local status

eb local open

eb local logs

eb local setenv

eb local printenv

## Options<a name="eb3-localoptions"></a>

eb local run


****  

|  Name  |  Description  | 
| --- | --- | 
|  `--envvars key1=value1,key2=value2`  |  Sets environment variables that the EB CLI will pass to the local Docker containers\. In multicontainer environments, all variables are passed to all containers\.  | 
|  `--port hostport`  |  Maps a port on the host to the exposed port on the container\. If you don't specify this option, the EB CLI uses the same port on both host and container\. This option works only with Docker platform applications\. It doesn't apply to the Multicontainer Docker platform\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

eb local status

eb local open

eb local logs

eb local setenv

eb local printenv


****  

|  Name  |  Description  | 
| --- | --- | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-localoutput"></a>

eb local run

Status messages from Docker\. Remains active as long as application is running\. Press **Ctrl\+C** to stop the application\.

eb local status

The status of each container used by the application, running or not\.

eb local open

Opens the application in a web browser and exits\.

eb local logs

The location of the logs generated in your project directory by applications running locally under eb local run\.

eb local setenv

None

eb local printenv

The name and values of environment variables set with eb local setenv\.

## Examples<a name="eb3-localexamples"></a>

eb local run

```
~/project$ eb local run
Creating elasticbeanstalk_phpapp_1...
Creating elasticbeanstalk_nginxproxy_1...
Attaching to elasticbeanstalk_phpapp_1, elasticbeanstalk_nginxproxy_1
phpapp_1     | [23-Apr-2015 23:24:25] NOTICE: fpm is running, pid 1
phpapp_1     | [23-Apr-2015 23:24:25] NOTICE: ready to handle connections
```

eb local status

View the status of your local containers:

```
~/project$ eb local status
Platform: 64bit Amazon Linux 2014.09 v1.2.1 running Multi-container Docker 1.3.3 (Generic)
Container name: elasticbeanstalk_nginxproxy_1
Container ip: 127.0.0.1
Container running: True
Exposed host port(s): 80
Full local URL(s): 127.0.0.1:80

Container name: elasticbeanstalk_phpapp_1
Container ip: 127.0.0.1
Container running: True
Exposed host port(s): None
Full local URL(s): None
```

eb local logs

View the log path for the current project:

```
~/project$ eb local logs
Elastic Beanstalk will write logs locally to /home/user/project/.elasticbeanstalk/logs/local.
Logs were most recently created 3 minutes ago and written to /home/user/project/.elasticbeanstalk/logs/local/150420_234011665784.
```

eb local setenv

Set environment variables for use with eb local run\.

```
~/project$ eb local setenv PARAM1=value
```

Print environment variables set with eb local setenv\.

```
~/project$ eb local printenv
Environment Variables:
PARAM1=value
```