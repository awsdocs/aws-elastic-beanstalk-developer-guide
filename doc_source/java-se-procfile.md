# Configuring the application process with a Procfile<a name="java-se-procfile"></a>

If you have more than one JAR file in the root of your application source bundle, you must include a `Procfile` file that tells Elastic Beanstalk which JAR\(s\) to run\. You can also include a `Procfile` file for a single JAR application to configure the Java virtual machine \(JVM\) that runs your application\.

We recommend that you always provide a `Procfile` in the source bundle alongside your application\. This way you precisely control which processes Elastic Beanstalk runs for your application and which arguments these processes receive\.

For details about writing and using a `Procfile`, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

**Example Procfile**  

```
web: java -jar server.jar -Xms256m
cache: java -jar mycache.jar
web_foo: java -jar other.jar
```

The command that runs the main JAR in your application must be called `web`, and it must be the first command listed in your `Procfile`\. The nginx server forwards all HTTP requests that it receives from your environment's load balancer to this application\.

Elastic Beanstalk assumes that all entries in the Procfile should run at all times and automatically restarts any application defined in the Procfile that terminates\. To run commands that will terminate and should not be restarted, use a [`Buildfile`](java-se-buildfile.md)\.

## Using a Procfile on Amazon Linux AMI \(preceding Amazon Linux 2\)<a name="java-se-procfile.alami"></a>

If your Elastic Beanstalk Java SE environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Port passing<a name="java-se-procfile.alami.ports"></a>

By default, Elastic Beanstalk configures the nginx proxy to forward requests to your application on port 5000\. You can override the default port by setting the `PORT` [environment property](java-se-platform.md#java-se-options) to the port on which your main application listens\.

If you use a `Procfile` to run multiple applications, Elastic Beanstalk on Amazon Linux AMI platform versions expects each additional application to listen on a port 100 higher than the previous one\. Elastic Beanstalk sets the PORT variable accessible from within each application to the port that it expects the application to run on\. You can access this variable within your application code by calling `System.getenv("PORT")`\.

In the preceding `Procfile` example, the `web` application listens on port 5000, `cache` listens on port 5100, and `web_foo` listens on port 5200\. `web` configures its listening port by reading the `PORT` variable, and adds 100 to that number to determine which port `cache` is listening on so that it can send it requests\.