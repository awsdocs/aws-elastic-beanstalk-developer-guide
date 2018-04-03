# Example: Using a Dockerfile to Customize and Configure a Preconfigured Docker Platform<a name="create_deploy_dockerpreconfig.dockerfile"></a>

With preconfigured Docker platforms you cannot use a configuration file to customize and configure the software that your application depends on\. Instead, if you want to customize the preconfigured Docker platform to install additional software packages that your application needs, you can add a `Dockerfile` to your application's root folder\.

You can include the following instructions in the `Dockerfile`:
+ **FROM** – \(required as the first instruction in the file\) Specifies the base image from which to build the Docker container and against which Elastic Beanstalk runs subsequent `Dockerfile` instructions\.

  The image can be hosted in a public repository, a private repository hosted by a third\-party registry, or a repository that you run on EC2\.
+ **EXPOSE** – \(required\) Lists the ports to expose on the Docker container\. Elastic Beanstalk uses the port value to connect the Docker container to the reverse proxy running on the host\.

  You can specify multiple container ports, but Elastic Beanstalk uses only the first one to connect your container to the host's reverse proxy and route requests from the public Internet\.
+ **CMD** – Specifies an executable and default parameters, which are combined into the command that the container runs at launch\. Use the following format:

  ```
  CMD ["executable","param1","param2"]
  ```

   `CMD` can also be used to provide default parameters for an `ENTRYPOINT` command by omitting the executable argument\. An executable must be specified in either a `CMD` or an `ENTRYPOINT`, but not both\. For basic scenarios, use a `CMD` and omit the `ENTRYPOINT`\. 
+  **ENTRYPOINT** – Uses the same JSON format as `CMD` and, like `CMD`, specifies a command to run when the container is launched\. Also allows a container to be run as an executable with `docker run`\.

   If you define an `ENTRYPOINT`, you can use a CMD as well to specify default parameters that can be overridden with `docker run`'s `-d` option\. The command defined by an `ENTRYPOINT` \(including any parameters\) is combined with parameters from `CMD` or `docker run` when the container is run\. 
+ **RUN** – Specifies one or more commands that install packages and configure your web application inside the image\.

  If you include RUN instructions in the `Dockerfile`, compress the file and the context used by RUN instructions in the `Dockerfile` into a `.zip` file\. Compress files at the top level of the directory\.

For more information about instructions you can include in the `Dockerfile`, go to [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/) on the Docker website\.

The following snippet is an example of a `Dockerfile`\. The instructions in the `Dockerfile` customize the Python 3\.4 platform by adding PostgreSQL dependencies and expose port 8080\.

**Note**  
Elastic Beanstalk preconfigured Docker platforms for Glassfish and Python require you to expose port 8080\. Elastic Beanstalk preconfigured Docker platforms for Go require you to expose port 3000\.

```
# Use the AWS Elastic Beanstalk Python 3.4 image
FROM amazon/aws-eb-python:3.4.2-onbuild-3.5.1

# Exposes port 8080
EXPOSE 8080

# Install PostgreSQL dependencies
RUN apt-get update && \
    apt-get install -y postgresql libpq-dev && \
    rm -rf /var/lib/apt/lists/*
```

If you want to use additional AWS resources \(such as Amazon DynamoDB or Amazon Simple Notification Service\), modify the proxy server or modify the operating system configuration for your Elastic Beanstalk environment\. For more information about using configuration files, see [AWS Elastic Beanstalk Environment Configuration](customize-containers.md)\.