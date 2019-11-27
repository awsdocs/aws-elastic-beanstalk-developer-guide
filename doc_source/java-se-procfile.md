# Configuring the Application Process with a Procfile<a name="java-se-procfile"></a>

If you have more than one JAR file in the root of your application source bundle, you must include a `Procfile` file that tells Elastic Beanstalk which JAR\(s\) to run\. You can also include a `Procfile` file for a single JAR application to configure the Java virtual machine \(JVM\) that runs your application\.

You must save the Procfile in your source bundle root\. The file name is case sensitive\. Format the Procfile as follows: a process name, followed by a colon, followed by a Java command that runs a JAR\. Each line in your Procfile must match the following regular expression: `^[A-Za-z0-9_]+:\s*.+$`\.

 **Procfile** 

```
web: java -jar server.jar -Xms256m
cache: java -jar mycache.jar
web_foo: java -jar other.jar
```

The command that runs the main JAR in your application must be called `web`, and it must be the first command listed in your `Procfile`\. The nginx server forwards all HTTP requests that it receives from your environment's load balancer to this application\.

By default, Elastic Beanstalk configures the nginx proxy to forward requests to your application on port 5000\. You can override the default port by setting the `PORT` [environment property](java-se-platform.md#java-se-options) to the port on which your main application listens\.

**Note**  
The port that your application listens on does not affect the port that the nginx server listens to receive requests from the load balancer\.

If you use a `Procfile` to run multiple applications, Elastic Beanstalk expects each additional application to listen on a port 100 higher than the previous one\. Elastic Beanstalk sets the PORT variable accessible from within each application to the port that it expects the application to run on\. You can access this variable within your application code by calling `System.getenv("PORT")`\.

**Note**  
In the preceding example, the `web` application listens on port 5000, `cache` listens on port 5100, and `web_foo` listens on port 5200\. `web` configures its listening port by reading the `PORT` variable, and adds 100 to that number to determine which port `cache` is listening on so that it can send it requests\.

Standard output and error streams from processes started with a `Procfile` are captured in log files named after the process and stored in `/var/log`\. For example, the `web` process in the preceding example generates logs named `web-1.log` and `web-1.error.log` for `stdout` and `stderr`, respectively\.

Elastic Beanstalk assumes that all entries in the Procfile should run at all times and automatically restarts any application defined in the Procfile that terminates\. To run commands that will terminate and should not be restarted, use a [`Buildfile`](java-se-buildfile.md)\.


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

All Amazon Linux 2 platforms support a uniform Procfile feature\. For the new Corretto platform versions running Amazon Linux 2, expand the *Procfile* section in [Extending Elastic Beanstalk Linux Platforms](platforms-linux-extend.md)\.