# Multicontainer Docker Environments with the AWS Management Console<a name="create_deploy_docker_ecstutorial"></a>

You can launch a cluster of multicontainer instances in a single\-instance or autoscaling Elastic Beanstalk environment using the AWS Management Console\. This tutorial details container configuration and source code preparation for an environment that uses two containers\. 

The containers, a PHP application and an nginx proxy, run side by side on each of the Amazon EC2 instances in an Elastic Beanstalk environment\. After creating the environment and verifying that the applications are running, you'll connect to a container instance to see how it all fits together\.


+ [Define Docker Containers](#create_deploy_docker_ecstutorial_config)
+ [Add Content](#create_deploy_docker_ecstutorial_code)
+ [Deploy to Elastic Beanstalk](#create_deploy_docker_ecstutorial_deploy)
+ [Connect to a Container Instance](#create_deploy_docker_ecstutorial_connect)
+ [Inspect the Amazon ECS Container Agent](#create_deploy_docker_ecstutorial_connect_inspect)

## Define Docker Containers<a name="create_deploy_docker_ecstutorial_config"></a>

The first step in creating a new Docker environment is to create a directory for your application data\. This folder can be located anywhere on your local machine and have any name you choose\. In addition to a container configuration file, this folder will contain the content that you will upload to Elastic Beanstalk and deploy to your environment\. 

**Note**  
All of the code for this tutorial is available in the awslabs repository on GitHub at [https://github\.com/awslabs/eb\-docker\-nginx\-proxy](https://github.com/awslabs/eb-docker-nginx-proxy)

The file that Elastic Beanstalk uses to configure the containers on an EC2 instance is a JSON\-formatted text file named `Dockerrun.aws.json`\. Create a text file with this name at the root of your application and add the following text: 

```
{
  "AWSEBDockerrunVersion": 2,
  "volumes": [
    {
      "name": "php-app",
      "host": {
        "sourcePath": "/var/app/current/php-app"
      }
    },
    {
      "name": "nginx-proxy-conf",
      "host": {
        "sourcePath": "/var/app/current/proxy/conf.d"
      }
    }  
  ],
  "containerDefinitions": [
    {
      "name": "php-app",
      "image": "php:fpm",
      "essential": true,
      "memory": 128,
      "mountPoints": [
        {
          "sourceVolume": "php-app",
          "containerPath": "/var/www/html",
          "readOnly": true
        }
      ]
    },
    {
      "name": "nginx-proxy",
      "image": "nginx",
      "essential": true,
      "memory": 128,
      "portMappings": [
        {
          "hostPort": 80,
          "containerPort": 80
        }
      ],
      "links": [
        "php-app"
      ],
      "mountPoints": [
        {
          "sourceVolume": "php-app",
          "containerPath": "/var/www/html",
          "readOnly": true
        },
        {
          "sourceVolume": "nginx-proxy-conf",
          "containerPath": "/etc/nginx/conf.d",
          "readOnly": true
        },
        {
          "sourceVolume": "awseb-logs-nginx-proxy",
          "containerPath": "/var/log/nginx"
        }
      ]
    }
  ]
}
```

This example configuration defines two containers, a PHP web site with an nginx proxy in front of it\. These two containers will run side by side in Docker containers on each instance in your Elastic Beanstalk environment, accessing shared content \(the content of the website\) from volumes on the host instance, which are also defined in this file\. The containers themselves are created from images hosted in official repositories on Docker Hub\. The resulting environment looks like the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-multicontainer-tutorial.png)

The volumes defined in the configuration correspond to the content that you will create next and upload as part of your application source bundle\. The containers access content on the host by mounting volumes in the `mountPoints` section of the container definitions\. 

For more information on the format of `Dockerrun.aws.config` and its parameters, see [Container Definition Format](create_deploy_docker_v2config.md#create_deploy_docker_v2config_dockerrun_format)\. 

## Add Content<a name="create_deploy_docker_ecstutorial_code"></a>

Next you will add some content for your PHP site to display to visitors, and a configuration file for the nginx proxy\. 

**php\-app\\index\.php**

```
<h1>Hello World!!!</h1>
<h3>PHP Version <pre><?= phpversion()?></pre></h3>
```

**php\-app\\static\.html**

```
<h1>Hello World!</h1>
<h3>This is a static HTML page.</h3>
```

**proxy\\conf\.d\\default\.conf**

```
server {
  listen 80;
  server_name localhost;
  root /var/www/html;
 
  index index.php;
 
  location ~ [^/]\.php(/|$) {
    fastcgi_split_path_info ^(.+?\.php)(/.*)$;
    if (!-f $document_root$fastcgi_script_name) {
      return 404;
    }

    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;

    fastcgi_pass php-app:9000;
    fastcgi_index index.php;
  }
}
```

## Deploy to Elastic Beanstalk<a name="create_deploy_docker_ecstutorial_deploy"></a>

Your application folder now contains the following files:

```
php-app\index.php
php-app\static.html
proxy\conf.d\default.conf
Dockerrun.aws.json
```

This is all you need to create the Elastic Beanstalk environment\. Create a `.zip` archive of the above files and folders \(not including the top\-level project folder\)\. To create the archive in Windows explorer, select the contents of the project folder, right\-click, select **Send To**, and then click **Compressed \(zipped\) Folder** 

**Note**  
For information on the required file structure and instructions for creating archives in other environments, see [Create an Application Source Bundle](applications-sourcebundle.md) 

Next, upload the source bundle to Elastic Beanstalk and create your environment\. When you are prompted to select a platform, choose **Multi\-container Docker**\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **App code**, choose **Upload**\.

1. Choose **Local file**, choose **Browse**, and open the source bundle\.

1. Choose **Upload**\.

1. Choose **Review and launch**\.

1. Review the available settings and choose **Create app**\.

The AWS Management Console redirects you to the management dashboard for your new environment\. This screen shows the health status of the environment and events output by the Elastic Beanstalk service\. When the status is Green, click the URL next to the environment name to see your new website\. 

## Connect to a Container Instance<a name="create_deploy_docker_ecstutorial_connect"></a>

So how does it all work? Next you will connect to an EC2 instance in your Elastic Beanstalk environment to see some of the moving parts in action\. 

First, identify the instance and note its public IP address, which is available in the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\. If multiple instances are running and you have trouble identifying the one the belongs to your environment, read through the events on the environment dashboard and find the instance ID\. This ID appears in an event listing when Elastic Beanstalk launches an EC2 instance\. Search for the instance ID in the Amazon EC2 console and view its details to find the public IP address\. 

Next, use an SSH client and your private key file to connect to the instance\. Use the following settings: 

**SSH Settings**

+ **Address** – The public IP address or DNS name of the EC2 instance\.

+ **Port** – **22**\. This port is opened for ingress by Elastic BeanstalkBeanstalk when you select an Amazon EC2 key pair during environment configuration\.

+ **User Name** – **ec2\-user**\. This is the default user name for EC2 instances running Amazon Linux\.

+ **Private Key** – Your private key file\.

For full instructions on using SSH to connect to an EC2 instance, see [ Connecting to Your Linux Instance Using SSH ](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

Now that your connected to the EC2 instance hosting your docker containers, you can see how things are set up\. Run `ls` on `/var/app/current`: 

```
[ec2-user@ip-10-0-0-117 ~]$ ls /var/app/current
Dockerrun.aws.json  php-app  proxy
```

This directory contains the files from the source bundle that you uploaded to Elastic Beanstalk during environment creation\. 

```
[ec2-user@ip-10-0-0-117 ~]$ ls /var/log/containers
nginx                             nginx-proxy-ffffd873ada5-stdouterr.log  rotated
nginx-66a4fd37eb63-stdouterr.log  php-app
nginx-proxy                       php-app-b894601a1364-stdouterr.log
```

This is where logs are created on the container instance and collected by Elastic Beanstalk\. Elastic Beanstalk creates a volume in this directory for each container, which you mount to the container location where logs are written\. 

You can also take a look at Docker to see the running containers with `docker ps`\. 

```
[ec2-user@ip-10-0-0-117 ~]$ sudo docker ps
CONTAINER ID        IMAGE                            COMMAND                CREATED             STATUS              PORTS                         NAMES
ffffd873ada5        nginx:1.7                        "nginx -g 'daemon of   About an hour ago   Up About an hour    443/tcp, 0.0.0.0:80->80/tcp   ecs-eb-dv-example-env-ycmk5geqrm-2-nginx-proxy-90fce996cc8cbecb2800
b894601a1364        php:5-fpm                        "php-fpm"              About an hour ago   Up About an hour    9000/tcp                      ecs-eb-dv-example-env-ycmk5geqrm-2-php-app-cec0918ed1a3a49a8001
09fb19828e38        amazon/amazon-ecs-agent:latest   "/agent"               About an hour ago   Up About an hour    127.0.0.1:51678->51678/tcp    ecs-agent
```

This shows the two running containers that you deployed, as well as the Amazon ECS container agent that coordinated the deployment\. 

## Inspect the Amazon ECS Container Agent<a name="create_deploy_docker_ecstutorial_connect_inspect"></a>

EC2 instances in a Multicontainer Docker environment on Elastic Beanstalk run an agent process in a Docker container\. This agent connects to the Amazon ECS service in order to coordinate container deployments\. These deployments run as tasks in Amazon ECS, which are configured in task definition files\. Elastic Beanstalk creates these task definition files based on the `Dockerrun.aws.json` that you upload in a source bundle\. 

Check the status of the container agent with an HTTP get request to `http://localhost:51678/v1/metadata`: 

```
[ec2-user@ip-10-0-0-117 ~]$ curl http://localhost:51678/v1/metadata
{
  "Cluster":"eb-dv-example-env-qpoxiguye24",
  "ContainerInstanceArn":"arn:aws:ecs:us-east-2:123456789012:container-instance/6a72af64-2838-400d-be09-3ab2d836ebcd"
}
```

This structure shows the name of the Amazon ECS cluster, and the ARN \([Amazon Resource Name](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\) of the cluster instance \(the EC2 instance that you are connected to\)\. 

For more information, make an HTTP get request to information is available at `http://localhost:51678/v1/tasks`:

```
[ec2-user@ip-10-0-0-117 ~]$ curl http://localhost:51678/v1/tasks
{
  "Tasks":[
    {
      "Arn":"arn:aws:ecs:us-east-2:123456789012:task/3ff2bf0f-790d-4f6d-affb-5b127b3b6e4a",
      "DesiredStatus":"RUNNING",
      "KnownStatus":"RUNNING",
      "Family":"eb-dv-example-env-qpoxiguye24",
      "Version":"2",
      "Containers":[
        {
          "DockerId":"b894601a1364a438156a239813c77cdef17040785bc4d5e49349470dc1556b15",
          "DockerName":"ecs-eb-dv-example-env-qpoxiguye24-2-php-app-cec0918ed1a3a49a8001",
          "Name":"php-app"
        },
        {
          "DockerId":"ffffd873ada5f537c88862cce4e1de7ec3edf962645982fb236961c833a5d0fe",
          "DockerName":"ecs-eb-dv-example-env-qpoxiguye24-2-nginx-proxy-90fce996cc8cbecb2800",
          "Name":"nginx-proxy"
        }
      ]
    }
  ]
}
```

This structure describes the task that is run to deploy the two docker containers from this tutorial's example project\. The following information is displayed: 

+ **KnownStatus** – The `RUNNING` status indicates that the containers are still active\.

+ **Family** – The name of the task definition that Elastic Beanstalk created from `Dockerrun.aws.json`\.

+ **Version** – The version of the task definition\. This is incremented each time the task definition file is updated\.

+ **Containers** – Information about the containers running on the instance\.

Even more information is available from the Amazon ECS service itself, which you can call using the AWS Command Line Interface\. For instructions on using the AWS CLI with Amazon ECS, and information about Amazon ECS in general, see the [ Amazon ECS User Guide](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_GetStarted.html)\. 