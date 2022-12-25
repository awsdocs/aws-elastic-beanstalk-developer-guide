# Docker configuration<a name="single-container-docker-configuration"></a>

This section describes how to prepare your Docker image and container for deployment to Elastic Beanstalk\. 

**Docker environment with Docker Compose**  
This section describes how to prepare your Docker image and container for deployment to Elastic Beanstalk\. Any web application that you deploy to Elastic Beanstalk in a Docker environment must include a `docker-compose.yml` file if you also use the Docker Compose tool\. You can deploy your web application as a containerized service to Elastic Beanstalk by doing one of the following actions:
+ Create a `docker-compose.yml` file to deploy a Docker image from a hosted repository to Elastic Beanstalk\. No other files are required if all your deployments are sourced from images in public repositories\. \(If your deployment must source an image from a private repository, you need to include additional configuration files for authentication\. For more information, see [Using images from a private repository](#docker-configuration.remote-repo)\.\) For more information about the `docker-compose.yml` file, see [Compose file reference](https://docs.docker.com/compose/compose-file/) on the Docker website\.
+  Create a `Dockerfile` to have Elastic Beanstalk build and run a custom image\. This file is optional, depending on your deployment requirements\. For more information about the `Dockerfile` see [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) on the Docker website\.
+ Create a `.zip` file containing your application files, any application file dependencies, the `Dockerfile`, and the `docker-compose.yml` file\. If you use the EB CLI to deploy your application, it creates a `.zip` file for you\. The two files must be at the root, or top level, of the `.zip` archive\.

  If you use only a `docker-compose.yml` file to deploy your application, you don't need to create a `.zip` file\.

This topic is a syntax reference\. For detailed procedures on launching Docker environments using Elastic Beanstalk, see [Using the Docker platform](docker.md)\.

To learn more about Docker Compose and how to install it, see the Docker sites [Overview of Docker Compose](https://docs.docker.com/compose/) and [Install Docker Compose](https://docs.docker.com/compose/install/)\.

**Note**  
If you don't use Docker Compose to configure your Docker environments, then you shouldn't use the `docker-compose.yml` file either\. Instead, use the `Dockerrun.aws.json` file or the `Dockerfile` or both\.  
For more information, see [Configuration for Docker platforms \(without Docker Compose\) ](#docker-configuration.no-compose)\.

## Using images from a private repository<a name="docker-configuration.remote-repo"></a>

Elastic Beanstalk must authenticate with the online registry that hosts the private repository before it can pull and deploy your images from a private repository\. You have two options to store and retrieve credentials for your Elastic Beanstalk environment to authenticate to a repository\. 
+  The AWS Systems Manager \(SSM\) Parameter Store 
+  The `Dockerrun.aws.json v3` file 

### Using the AWS Systems Manager \(SSM\) Parameter Store<a name="docker-configuration.remote-repo.ssm"></a>

You can configure Elastic Beanstalk to log in to your private repository before it starts the deployment process\. This enables Elastic Beanstalk to access the images from the repository and deploy these images to your Elastic Beanstalk environment\. 

This configuration initiates events in the *prebuild* phase of the Elastic Beanstalk deployment process\. You set this up in the [\.ebextentions](ebextensions.md) configuration directory\. The configuration uses [platform hook](platforms-linux-extend.md#platforms-linux-extend.hooks) scripts that call docker login to authenticate to the online registry that hosts the private repository\. A detailed breakdown of these configuration steps follows\. 

**To configure Elastic Beanstalk to authenticate to your private repository with AWS SSM**
**Note**  
You need to set up AWS Systems Manager to complete these steps\. For more information, see the [AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/getting-started.html)

1. Create your `.ebextensions` directory structure as follows\.

   ```
   ├── .ebextensions
   │   └── env.config
   ├── .platform
   │   ├── confighooks
   │   │   └── prebuild
   │   │       └── 01login.sh
   │   └── hooks
   │       └── prebuild
   │           └── 01login.sh
   ├── docker-compose.yml
   ```

1. Use the [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/getting-started.html) Parameter Store to save the credentials of your private repository so that Elastic Beanstalk can retrieve your credentials when required\. For this, run the [https://docs.aws.amazon.com/cli/latest/reference/ssm/put-parameter.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/put-parameter.html) command\. 

   ```
   aws ssm put-parameter --name USER --type String --value "username"
   aws ssm put-parameter --name PASSWD --type String --value "passwd"
   ```

1.  Create the following `env.config` file and place it in the `.ebextensions` directory as shown in the preceding directory structure\. 
**Note**  
`USER` and `PASSWD` in the script must match the same strings that are used in the preceding ssm put\-parameter commands\.

   ```
   option_settings:
     aws:elasticbeanstalk:application:environment:
       USER: '{{resolve:ssm:USER:1}}'
       PASSWD: '{{resolve:ssm:PASSWD:1}}'
   ```

1. Create the following `01login.sh` script file and place it in the following directories \(also shown in the preceding directory structure\):
   + `.platform/confighooks/prebuild`
   + `.platform/hooks/prebuild`

   ```
   ### example 01login.sh
   #!/bin/bash
   USER=/opt/elasticbeanstalk/bin/get-config environment -k USER
   PASSWD=/opt/elasticbeanstalk/bin/get-config environment -k PASSWD
   docker login -u $USER -p $PASSWD
   ```

   The `01login.sh` script first calls the get\-config tool to retrieve the repository credentials, and then calls docker login to authenticate to the repository\. 
**Notes**  
All script files must have execute permission\. Use chmod \+x to set execute permission on your hook files\.
Hook files can be either binary files or script files starting with a \#\! line containing their interpreter path, such as \#\!/bin/bash\.
For more information, see [Platform hooks](platforms-linux-extend.md#platforms-linux-extend.hooks) in *Extending Elastic Beanstalk Linux platforms*\.

After Elastic Beanstalk can authenticate with the online registry that hosts the private repository, your images can be deployed and pulled\.

### Using the `Dockerrun.aws.json v3` file<a name="docker-configuration.remote-repo.dockerrun-aws"></a>

This section describes another approach to authenticate Elastic Beanstalk to a private repository\. With this approach, you generate an authentication file with the Docker command, and then upload the authentication file to an Amazon S3 bucket\. You must also include the bucket information in your `Dockerrun.aws.json v3` file\.

**To generate and provide an authentication file to Elastic Beanstalk**

1. Generate an authentication file with the docker login command\. For repositories on Docker Hub, run docker login:

   ```
   $ docker login
   ```

   For other registries, include the URL of the registry server:

   ```
   $ docker login registry-server-url
   ```
**Note**  
If your Elastic Beanstalk environment uses the Amazon Linux AMI Docker platform version \(precedes Amazon Linux 2\), read the relevant information in [Docker configuration on Amazon Linux AMI \(preceding Amazon Linux 2\)](create_deploy_docker.container.console.md#docker-alami)\.

   For more information about the authentication file, see [ Store images on Docker Hub ](https://docs.docker.com/docker-hub/repos/) and [ docker login ](https://docs.docker.com/engine/reference/commandline/login/) on the Docker website\.

1. Upload a copy of the authentication file that is named `.dockercfg` to a secure Amazon S3 bucket\.
   + The Amazon S3 bucket must be hosted in the same AWS Region as the environment that is using it\. Elastic Beanstalk cannot download files from an Amazon S3 bucket hosted in other Regions\.
   + Grant permissions for the `s3:GetObject` operation to the IAM role in the instance profile\. For more information, see [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)\.

1. Include the Amazon S3 bucket information in the `Authentication` parameter in your `Dockerrun.aws.json v3` file\.

   Following is an example of a `Dockerrun.aws.json v3` file\.

   ```
   {
     "AWSEBDockerrunVersion": "3",
     "Authentication": {
       "bucket": "DOC-EXAMPLE-BUCKET",
       "key": "mydockercfg"
     }
   }
   ```
**Note**  
 The `AWSEBDockerrunVersion` parameter indicates the version of the `Dockerrun.aws.json` file\.  
 The Docker Amazon Linux 2 platform uses the `Dockerrun.aws.json v3` file for environments that use Docker Compose\. It uses the `Dockerrun.aws.json v1` file for environments that don't use Docker Compose\. 
 The Multicontainer Docker Amazon Linux AMI platform uses the `Dockerrun.aws.json v2` file\. 

After Elastic Beanstalk can authenticate with the online registry that hosts the private repository, your images can be deployed and pulled\.

## Building custom images with a Dockerfile<a name="single-container-docker-configuration.dockerfile"></a>

You need to create a `Dockerfile` if you don't already have an existing image hosted in a repository\. 

The following snippet is an example of the `Dockerfile`\. If you follow the instructions in [Using the Docker platform](docker.md), you can upload this `Dockerfile` as written\. Elastic Beanstalk runs the game 2048 when you use this `Dockerfile`\.

```
FROM ubuntu:12.04

RUN apt-get update
RUN apt-get install -y nginx zip curl

RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN curl -o /usr/share/nginx/www/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master
RUN cd /usr/share/nginx/www/ && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip

EXPOSE 80

CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]
```

For more information about instructions you can include in the `Dockerfile`, see [Dockerfile reference](https://docs.docker.com/engine/reference/builder) on the Docker website\.

## Configuration for Docker platforms \(without Docker Compose\)<a name="docker-configuration.no-compose"></a>

If your Elastic Beanstalk Docker environment does not use Docker Compose, read the additional information in the following sections\.

### Docker platform Configuration \- without Docker Compose<a name="single-container-docker-configuration.no-compose"></a>

Any web application that you deploy to Elastic Beanstalk in a Docker environment must include either a `Dockerfile` or a `Dockerrun.aws.json` file\. You can deploy your web application from a Docker container to Elastic Beanstalk by doing one of the following actions:
+ Create a `Dockerfile` to have Elastic Beanstalk build and run a custom image\.
+ Create a `Dockerrun.aws.json` file to deploy a Docker image from a hosted repository to Elastic Beanstalk\.
+ Create a `.zip` file containing your application files, any application file dependencies, the `Dockerfile`, and the `Dockerrun.aws.json` file\. If you use the EB CLI to deploy your application, it creates a `.zip` file for you\.

  If you use only a `Dockerfile` or only a `Dockerrun.aws.json` file to deploy your application, you don't need to create a `.zip` file\.

This topic is a syntax reference\. For detailed procedures on launching Docker environments, see [Using the Docker platform](docker.md)\.

**Topics**

#### `Dockerrun.aws.json` v1<a name="single-container-docker-configuration.dockerrun"></a>

A `Dockerrun.aws.json` file describes how to deploy a remote Docker image as an Elastic Beanstalk application\. This JSON file is specific to Elastic Beanstalk\. If your application runs on an image that is available in a hosted repository, you can specify the image in a `Dockerrun.aws.json v1` file and omit the `Dockerfile`\.



Valid keys and values for the `Dockerrun.aws.json v1` file include the following operations:

**AWSEBDockerrunVersion**  
\(Required\) Specifies the version number as the value `1` for single container Docker environments\.

**Authentication**  
\(Required only for private repositories\) Specifies the Amazon S3 object storing the `.dockercfg` file\.  
See [ Using images from a private repository](#single-container-docker-configuration.privaterepo)\.

**Image**  
Specifies the Docker base image on an existing Docker repository from which you're building a Docker container\. Specify the value of the **Name** key in the format *<organization>/<image name>* for images on Docker Hub, or *<site>/<organization name>/<image name>* for other sites\.   
When you specify an image in the `Dockerrun.aws.json` file, each instance in your Elastic Beanstalk environment runs `docker pull` to run the image\. Optionally, include the **Update** key\. The default value is `true` and instructs Elastic Beanstalk to check the repository, pull any updates to the image, and overwrite any cached images\.  
When using a `Dockerfile`, do not specify the **Image** key in the `Dockerrun.aws.json` file\. Elastic Beanstalk always builds and uses the image described in the `Dockerfile` when one is present\.

**Ports**  
\(Required when you specify the **Image** key\) Lists the ports to expose on the Docker container\. Elastic Beanstalk uses the **ContainerPort** value to connect the Docker container to the reverse proxy running on the host\.  
You can specify multiple container ports, but Elastic Beanstalk uses only the first port\. It uses this port to connect your container to the host's reverse proxy and route requests from the public internet\. If you're using a `Dockerfile`, the first **ContainerPort** value should match the first entry in the `Dockerfile`'s **EXPOSE** list\.   
Optionally, you can specify a list of ports in **HostPort**\. **HostPort** entries specify the host ports that **ContainerPort** values are mapped to\. If you don't specify a **HostPort** value, it defaults to the **ContainerPort** value\.   

```
{
  "Image": {
    "Name": "image-name"
  },
  "Ports": [
    {
      "ContainerPort": 8080,
      "HostPort": 8000
    }
  ]
}
```

****Volumes****  
Map volumes from an EC2 instance to your Docker container\. Specify one or more arrays of volumes to map\.  

```
{
  "Volumes": [
    {
      "HostDirectory": "/path/inside/host",
      "ContainerDirectory": "/path/inside/container"
    }
  ]
...
```

****Logging****  
Specify the directory inside the container to which your application writes logs\. Elastic Beanstalk uploads any logs in this directory to Amazon S3 when you request tail or bundle logs\. If you rotate logs to a folder named `rotated` within this directory, you can also configure Elastic Beanstalk to upload rotated logs to Amazon S3 for permanent storage\. For more information, see [Viewing logs from Amazon EC2 instances in your Elastic Beanstalk environment](using-features.logging.md)\.

**Command**  
Specify a command to run in the container\. If you specify an **Entrypoint**, then **Command** is added as an argument to **Entrypoint**\. For more information, see [CMD](https://docs.docker.com/engine/reference/run/#cmd-default-command-or-options) in the Docker documentation\.

**Entrypoint**  
Specify a default command to run when the container starts\. For more information, see [ENTRYPOINT](https://docs.docker.com/engine/reference/run/#cmd-default-command-or-options) in the Docker documentation\.

The following snippet is an example that illustrates the syntax of the `Dockerrun.aws.json` file for a single container\.

```
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "janedoe/image",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "1234"
    }
  ],
  "Volumes": [
    {
      "HostDirectory": "/var/app/mydb",
      "ContainerDirectory": "/etc/mysql"
    }
  ],
  "Logging": "/var/log/nginx",
  "Entrypoint": "/app/bin/myapp",
  "Command": "--argument"
}
```

You can provide Elastic Beanstalk with only the `Dockerrun.aws.json` file, or with a `.zip` archive containing both the `Dockerrun.aws.json` and `Dockerfile` files\. When you provide both files, the `Dockerfile` describes the Docker image and the `Dockerrun.aws.json` file provides additional information for deployment, as described later in this section\.

**Note**  
The two files must be at the root, or top level, of the `.zip` archive\. Don't build the archive from a directory containing the files\. Instead, navigate into that directory and build the archive there\.  
When you provide both files, don't specify an image in the `Dockerrun.aws.json` file\. Elastic Beanstalk builds and uses the image described in the `Dockerfile` and ignores the image specified in the `Dockerrun.aws.json` file\.

#### Using images from a private repository<a name="single-container-docker-configuration.privaterepo"></a>

Add the information about the Amazon S3 bucket that contains the authentication file in the `Authentication` parameter of the `Dockerrun.aws.json v1` file\. Make sure that the `Authentication` parameter contains a valid Amazon S3 bucket and key\. The Amazon S3 bucket must be hosted in the same AWS Region as the environment that is using it\. Elastic Beanstalk doesn't download files from Amazon S3 buckets hosted in other Regions\. 

For information about generating and uploading the authentication file, see [Using images from a private repository](create_deploy_docker.container.console.md#docker-images-private)\.

The following example shows the use of an authentication file named `mydockercfg` in a bucket named `DOC-EXAMPLE-BUCKET` to use a private image in a third\-party registry\.

```
{
  "AWSEBDockerrunVersion": "1",
  "Authentication": {
    "Bucket": "DOC-EXAMPLE-BUCKET",
    "Key": "mydockercfg"
  },
  "Image": {
    "Name": "quay.io/johndoe/private-image",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "1234"
    }
  ],
  "Volumes": [
    {
      "HostDirectory": "/var/app/mydb",
      "ContainerDirectory": "/etc/mysql"
    }
  ],
  "Logging": "/var/log/nginx"
}
```