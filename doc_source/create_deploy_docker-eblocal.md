# Running a Docker environment locally with the EB CLI<a name="create_deploy_docker-eblocal"></a>

You can use the Elastic Beanstalk Command Line Interface \(EB CLI\) to run the Docker containers configured in your AWS Elastic Beanstalk application locally\. The EB CLI uses the Docker configuration file \(Dockerfile or Dockerrun\.aws\.json\) and source code in your project directory to run your application locally in Docker\.

The EB CLI supports locally running applications defined using the Docker, Multicontainer Docker, and Preconfigured Docker platforms\.

**Topics**
+ [Prerequisites for running Docker applications locally](#create_deploy_docker-eblocal-prereqs)
+ [Preparing a Docker application for use with the EB CLI](#create_deploy_docker-eblocal-prepare)
+ [Running a Docker application locally](#create_deploy_docker-eblocal-localrun)
+ [Cleaning up after running a Docker application locally](#create_deploy_docker-eblocal-cleanup)

## Prerequisites for running Docker applications locally<a name="create_deploy_docker-eblocal-prereqs"></a>
+ Linux OS or Mac OS X
+ [EB CLI version 3\.3 or greater](eb-cli3-install.md)

  Run eb init in your project directory to initialize an EB CLI repository\. If you haven't used the EB CLI before, see [Managing Elastic Beanstalk environments with the EB CLI](eb-cli3-getting-started.md)\.
+ [Docker version 1\.6 or greater](https://docs.docker.com/engine/installation/)

  Add yourself to the `docker` group, log out, and then log back in to ensure that you can run Docker commands without `sudo`:

  ```
  $ sudo usermod -a -G docker $USER
  ```

  Run `docker ps` to verify that the Docker daemon is up and running:

  ```
  $ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  ```
+ A Docker application

  If you don't have a Docker application in a project folder on your local machine, see [Deploying Elastic Beanstalk applications from Docker containers](create_deploy_docker.md) for an introduction to using Docker with AWS Elastic Beanstalk\.
+ Docker profile \(optional\)

  If your application uses Docker images that are in a private repository, run `docker login` and follow the prompts to create an authentication profile\.
+ w3m \(optional\)

  W3m is a web browser that you can use to view your running web application within a command line terminal with eb local run\. If you are using the command line in a desktop environment, you don't need w3m\.

Docker containers run locally without emulating AWS resources that are provisioned when you deploy an application to Elastic Beanstalk, including security groups and data or worker tiers\.

You can configure your local containers to connect to a database by passing the necessary connection string or other variables with the `envvars` option, but you must ensure that any resources in AWS are accessible from your local machine by [opening the appropriate ports](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html) in their assigned security groups or attaching a [default gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) or [elastic IP address](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)\.

## Preparing a Docker application for use with the EB CLI<a name="create_deploy_docker-eblocal-prepare"></a>

Prepare your Docker configuration file and source data as though you were deploying them to Elastic Beanstalk\. This topic uses the PHP and nginx proxy example from the [Multicontainer Docker tutorial](create_deploy_docker_ecstutorial.md) earlier in this guide as an example, but you can use the same commands with any Docker, Multicontainer Docker, or Preconfigured Docker application\.

## Running a Docker application locally<a name="create_deploy_docker-eblocal-localrun"></a>

Run your Docker application locally with the eb local run command from within the project directory:

```
~/project$ eb local run
Creating elasticbeanstalk_phpapp_1...
Creating elasticbeanstalk_nginxproxy_1...
Attaching to elasticbeanstalk_phpapp_1, elasticbeanstalk_nginxproxy_1
phpapp_1     | [23-Apr-2015 23:24:25] NOTICE: fpm is running, pid 1
phpapp_1     | [23-Apr-2015 23:24:25] NOTICE: ready to handle connections
```

The EB CLI reads the Docker configuration and executes the Docker commands necessary to run your application\. The first time you run a project locally, Docker downloads images from a remote repository and stores them on your local machine\. This process can take several minutes\.

**Note**  
The eb local run command takes two optional parameters, `port` and `envvars`\.  
To override the default port for a Docker application, use the `port` option:  

```
$ eb local run --port 8080
```
This command tells the EB CLI to use port 8080 on the host and map it to the exposed port on the container\. If you don't specify a port, the EB CLI uses the container's port for the host\. This option only works with applications using the Docker platform\.  
To pass environment variables to the application containers, use the `envvars` option:  

```
$ eb local run --envvars RDS_HOST=$RDS_HOST,RDS_DB=$RDS_DB,RDS_USER=$RDS_USER,RDS_PASS=$RDS_PASS
```
Use environment variables to configure a database connection, set debug options, or pass secrets securely to your application\. For more information on the options supported by the eb local subcommands, see [eb local](eb3-local.md)\.

After the containers are up and running in Docker, they are ready to take requests from clients\. The eb local process stays open as long as the containers are running\. If you need to stop the process and containers, press **Ctrl\+C**\.

Open a second terminal to run additional commands while the eb local process is running\. Use eb local status to view your application's status:

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

You can use `docker ps` to see the status of the containers from Docker's point of view:

```
~/project$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                         NAMES
6a8e71274fed        nginx:latest        "nginx -g 'daemon of   9 minutes ago       Up 9 minutes        0.0.0.0:80->80/tcp, 443/tcp   elasticbeanstalk_nginxproxy_1
82cbf620bdc1        php:fpm             "php-fpm"              9 minutes ago       Up 9 minutes        9000/tcp                      elasticbeanstalk_phpapp_1
```

Next, view your application in action with eb local open:

```
~/project$ eb local open
```

This command opens your application in the default web browser\. If you are running a terminal in a desktop environment, this may be Firefox, Safari, or Google Chrome\. If you are running a terminal in a headless environment or over an SSH connection, a command line browser, such as w3m, will be used if one is available\.

Switch back to the terminal running the application process for a moment and note the additional output:

```
phpapp_1     | 172.17.0.36 -  21/Apr/2015:23:46:17 +0000 "GET /index.php" 200
```

This shows that the web application in the Docker container received an HTTP GET request for index\.php that was returned successfully with a 200 \(non error\) status\.

Run eb local logs to see where the EB CLI writes the logs\.

```
~/project$ eb local logs
Elastic Beanstalk will write logs locally to /home/user/project/.elasticbeanstalk/logs/local.
Logs were most recently created 3 minutes ago and written to /home/user/project/.elasticbeanstalk/logs/local/150420_234011665784.
```

## Cleaning up after running a Docker application locally<a name="create_deploy_docker-eblocal-cleanup"></a>

When you are done testing your application locally, you can stop the applications and remove the images downloaded by Docker when you use eb local run\. Removing the images is optional\. You may want to keep them for future use\.

Return to the terminal running the eb local process and press **Ctrl\+C**to stop the application: 

```
^CGracefully stopping... (press Ctrl+C again to force)
Stopping elasticbeanstalk_nginxproxy_1...
Stopping elasticbeanstalk_phpapp_1...

Aborting.
[1]+  Exit 5                  eb local run
```

The EB CLI attempts to stop each running container gracefully with Docker commands\. If you need to stop a process immediately, press **Ctrl\+C** again\.

After you stop the applications, the Docker containers should also stop running\. Verify this with `docker ps`:

```
$ docker ps --all
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                      PORTS               NAMES
73d515d99d2a        nginx:latest        "nginx -g 'daemon of   21 minutes ago      Exited (0) 11 minutes ago                       elasticbeanstalk_nginxproxy_1
7061c76220de        php:fpm             "php-fpm"              21 minutes ago      Exited (0) 11 minutes ago                       elasticbeanstalk_phpapp_1
```

The `all` option shows stopped containers \(if you omitted this option, the output will be blank\)\. In the above example, Docker shows that both containers exited with a 0 \(non\-error\) status\.

If you are done using Docker and EB CLI local commands, you can remove the Docker images from your local machine to save space\. 

**To remove Docker images from your local machine**

1. View the images that you downloaded using `docker images`:

   ```
   $ docker images
   REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
   php                 fpm                 68bc5150cffc        1 hour ago          414.1 MB
   nginx               latest              637d3b2f5fb5        1 hour ago          93.44 MB
   ```

1. Remove the two Docker containers with `docker rm`:

   ```
   $ docker rm 73d515d99d2a 7061c76220de
   73d515d99d2a
   7061c76220de
   ```

1. Remove the images with `docker rmi`:

   ```
   $ docker rmi 68bc5150cffc 637d3b2f5fb5
   Untagged: php:fpm
   Deleted: 68bc5150cffc0526c66b92265c3ed8f2ea50f3c71d266aa655b7a4d20c3587b0
   Untagged: nginx:latest
   Deleted: 637d3b2f5fb5c4f70895b77a9e76751a6e7670f4ef27a159dad49235f4fe61e0
   ```