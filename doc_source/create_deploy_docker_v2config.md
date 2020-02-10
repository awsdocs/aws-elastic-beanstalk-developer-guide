# Multicontainer Docker configuration<a name="create_deploy_docker_v2config"></a>

A `Dockerrun.aws.json` file is an Elastic Beanstalk–specific JSON file that describes how to deploy a set of Docker containers as an Elastic Beanstalk application\. You can use a `Dockerrun.aws.json` file for a multicontainer Docker environment\. 

`Dockerrun.aws.json` describes the containers to deploy to each container instance \(Amazon EC2 instance that hosts Docker containers\) in the environment as well as the data volumes to create on the host instance for the containers to mount\.

A `Dockerrun.aws.json` file can be used on its own or zipped up with additional source code in a single archive\. Source code that is archived with a `Dockerrun.aws.json` is deployed to Amazon EC2 container instances and accessible in the `/var/app/current/` directory\. Use the `volumes` section of the config to provide file volumes for the Docker containers running on the host instance\. Use the `mountPoints` section of the embedded container definitions to map these volumes to mount points that applications on the Docker containers can use\.

**Topics**
+ [Dockerrun\.aws\.json v2](#create_deploy_docker_v2config_dockerrun)
+ [Using images from a private repository](#docker-multicontainer-dockerrun-privaterepo)
+ [Container definition format](#create_deploy_docker_v2config_dockerrun_format)

## Dockerrun\.aws\.json v2<a name="create_deploy_docker_v2config_dockerrun"></a>

The `Dockerrun.aws.json` file includes three sections:

**AWSEBDockerrunVersion**  
Specifies the version number as the value `2` for multicontainer Docker environments\.

**containerDefinitions**  
An array of container definitions, detailed below\.

**volumes**  
Creates volumes from folders in the Amazon EC2 container instance, or from your source bundle \(deployed to `/var/app/current`\)\. Mount these volumes to paths within your Docker containers using `mountPoints` in the [container definition](#create_deploy_docker_v2config_dockerrun_format)\.   
 Elastic Beanstalk configures additional volumes for logs, one for each container\. These should be mounted by your Docker containers in order to write logs to the host instance\. See [Container definition format](#create_deploy_docker_v2config_dockerrun_format) for details\. 
 Volumes are specified in the following format:   

```
"volumes": [
    {
      "name": "volumename",
      "host": {
        "sourcePath": "/path/on/host/instance"
      }
    }
  ],
```

**authentication**  
\(optional\) The location in Amazon S3 of a `.dockercfg` file that contains authentication data for a private repository\. Uses the following format:  

```
"authentication": {
    "bucket": "my-bucket",
    "key": "mydockercfg"
  },
```
See [Using images from a private repository](#docker-multicontainer-dockerrun-privaterepo) for details\. 

The following snippet is an example that illustrates the syntax of the `Dockerrun.aws.json` file for an instance with two containers\.

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
      "environment": [
        {
          "name": "Container",
          "value": "PHP"
        }
      ],
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

## Using images from a private repository<a name="docker-multicontainer-dockerrun-privaterepo"></a>

Add the information about the Amazon S3 bucket that contains the authentication file in the `authentication` parameter of the `Dockerrun.aws.json` file\. Make sure that the `authentication` parameter contains a valid Amazon S3 bucket and key\. The Amazon S3 bucket must be hosted in the same region as the environment that is using it\. Elastic Beanstalk will not download files from Amazon S3 buckets hosted in other regions\. 

For information about generating and uploading the authentication file, see [Using images from a private repository](create_deploy_docker.container.console.md#docker-images-private)\.

## Container definition format<a name="create_deploy_docker_v2config_dockerrun_format"></a>

The container definition and volumes sections of `Dockerrun.aws.json` use the same formatting as the corresponding sections of an Amazon ECS task definition file\.

The following examples show a subset of parameters that are commonly used\. More optional parameters are available\. For more information on the task definition format and a full list of task definition parameters, see [Amazon ECS Task Definitions](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_defintions.html) in the *Amazon Elastic Container Service Developer Guide*\.

A `Dockerrun.aws.json` file contains an array of one or more container definition objects with the following fields:

**name**  
The name of the container\. See [Standard Container Definition Parameters](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#standard_container_definition_params) for information about the maximum length and allowed characters\.

**image**  
The name of a Docker image in an online Docker repository from which you're building a Docker container\. Note these conventions:   
+  Images in official repositories on Docker Hub use a single name \(for example, `ubuntu` or `mongo`\)\.
+ Images in other repositories on Docker Hub are qualified with an organization name \(for example, `amazon/amazon-ecs-agent`\.
+ Images in other online repositories are qualified further by a domain name \(for example, `quay.io/assemblyline/ubuntu`\)\.

**environment**  
An array of environment variables to pass to the container\.  
For example, the following entry defines an environment variable with the name **Container** and the value **PHP**:  

```
"environment": [
  {
    "name": "Container",
    "value": "PHP"
  }
],
```

**essential**  
True if the task should stop if the container fails\. Nonessential containers can finish or crash without affecting the rest of the containers on the instance\. 

**memory**  
Amount of memory on the container instance to reserve for the container\. Specify a non\-zero integer for one or both of the `memory` or `memoryReservation` parameters in container definitions\.

**memoryReservation**  
The soft limit \(in MiB\) of memory to reserve for the container\. Specify a non\-zero integer for one or both of the `memory` or `memoryReservation` parameters in container definitions\.

**mountPoints**  
Volumes from the Amazon EC2 container instance to mount, and the location on the Docker container file system at which to mount them\. When you mount volumes that contain application content, your container can read the data you upload in your source bundle\. When you mount log volumes for writing log data, Elastic Beanstalk can gather log data from these volumes\.   
 Elastic Beanstalk creates log volumes on the container instance, one for each Docker container, at `/var/log/containers/containername`\. These volumes are named `awseb-logs-containername` and should be mounted to the location within the container file structure where logs are written\.   
For example, the following mount point maps the nginx log location in the container to the Elastic Beanstalk–generated volume for the `nginx-proxy` container\.   

```
{
  "sourceVolume": "awseb-logs-nginx-proxy",
  "containerPath": "/var/log/nginx"
}
```

**portMappings**  
Maps network ports on the container to ports on the host\.

**links**  
List of containers to link to\. Linked containers can discover each other and communicate securely\. 

**volumesFrom**  
Mount all of the volumes from a different container\. For example, to mount volumes from a container named `web`:  

```
"volumesFrom": [
  {
    "sourceContainer": "web"
  }
],
```