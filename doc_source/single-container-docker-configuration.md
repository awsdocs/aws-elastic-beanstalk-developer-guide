# Docker configuration<a name="single-container-docker-configuration"></a>

This section describes how to prepare your Docker image and container for deployment to Elastic Beanstalk\. Any web application that you deploy to Elastic Beanstalk in a single container Docker environment must include a `Dockerfile` or a `Dockerrun.aws.json` file\. You can deploy your web application from a Docker container to Elastic Beanstalk by doing one of the following:
+ Create a `Dockerfile` to have Elastic Beanstalk build and run a custom image\.
+ Create a `Dockerrun.aws.json` file to deploy a Docker image from a hosted repository to Elastic Beanstalk\.
+ Create a `.zip` file containing your application files, any application file dependencies, the `Dockerfile`, and the `Dockerrun.aws.json` file\. If you use the EB CLI to deploy your application, it will create a `.zip` file for you\.

  If you use only a `Dockerfile` or only a `Dockerrun.aws.json` file to deploy your application, you don't need to create a `.zip` file\.

This topic is a syntax reference\. For detailed procedures on launching Docker environments, see [Using the Docker platform](single-container-docker.md)\.

**Topics**
+ [Dockerrun\.aws\.json v1](#single-container-docker-configuration.dockerrun)
+ [Using images from a private repository](#single-container-docker-configuration.privaterepo)
+ [Building custom images with a Dockerfile](#single-container-docker-configuration.dockerfile)

## Dockerrun\.aws\.json v1<a name="single-container-docker-configuration.dockerrun"></a>

A `Dockerrun.aws.json` file describes how to deploy a remote Docker image as an Elastic Beanstalk application\. This JSON file is specific to Elastic Beanstalk\. If your application runs on an image that is available in a hosted repository, you can specify the image in a `Dockerrun.aws.json` file and omit the `Dockerfile`\.

Valid keys and values for the `Dockerrun.aws.json` file include the following:

**AWSEBDockerrunVersion**  
\(Required\) Specifies the version number as the value `1` for single container Docker environments\.

**Authentication**  
\(Required only for private repositories\) Specifies the Amazon S3 object storing the `.dockercfg` file\.  
See [Using images from a private repository](#single-container-docker-configuration.privaterepo)\.

**Image**  
Specifies the Docker base image on an existing Docker repository from which you're building a Docker container\. Specify the value of the **Name** key in the format *<organization>/<image name>* for images on Docker Hub, or *<site>/<organization name>/<image name>* for other sites\.   
When you specify an image in the `Dockerrun.aws.json` file, each instance in your Elastic Beanstalk environment will run `docker pull` on that image and run it\. Optionally, include the **Update** key\. The default value is `true` and instructs Elastic Beanstalk to check the repository, pull any updates to the image, and overwrite any cached images\.  
When using a `Dockerfile`, do not specify the **Image** key in the `Dockerrun.aws.json` file\. Elastic Beanstalk always builds and uses the image described in the `Dockerfile` when one is present\.

**Ports**  
\(Required when you specify the **Image** key\) Lists the ports to expose on the Docker container\. Elastic Beanstalk uses the **ContainerPort** value to connect the Docker container to the reverse proxy running on the host\.  
You can specify multiple container ports, but Elastic Beanstalk uses only the first one to connect your container to the host's reverse proxy and route requests from the public internet\. If you're using a `Dockerfile`, the first **ContainerPort** value should match the first entry in the `Dockerfile`'s **EXPOSE** list\.   
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
Specify a command to execute in the container\. If you specify an **Entrypoint**, then **Command** is added as an argument to **Entrypoint**\. For more information, see [CMD](https://docs.docker.com/engine/reference/run/#cmd-default-command-or-options) in the Docker documentation\.

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
The two files must be at the root, or top level, of the `.zip` archive\. Do not build the archive from a directory containing the files\. Navigate into that directory and build the archive there\.  
When you provide both files, do not specify an image in the `Dockerrun.aws.json` file\. Elastic Beanstalk builds and uses the image described in the `Dockerfile` and ignores the image specified in the `Dockerrun.aws.json` file\.

## Using images from a private repository<a name="single-container-docker-configuration.privaterepo"></a>

Add the information about the Amazon S3 bucket that contains the authentication file in the `Authentication` parameter of the `Dockerrun.aws.json` file\. Make sure that the `Authentication` parameter contains a valid Amazon S3 bucket and key\. The Amazon S3 bucket must be hosted in the same AWS Region as the environment that is using it\. Elastic Beanstalk will not download files from Amazon S3 buckets hosted in other Regions\. 

For information about generating and uploading the authentication file, see [Using images from a private repository](create_deploy_docker.container.console.md#docker-images-private)\.

The following example shows the use of an authentication file named `mydockercfg` in a bucket named `my-bucket` to use a private image in a third\-party registry\.

```
{
  "AWSEBDockerrunVersion": "1",
  "Authentication": {
    "Bucket": "my-bucket",
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

## Building custom images with a Dockerfile<a name="single-container-docker-configuration.dockerfile"></a>

Create a `Dockerfile` when you don't already have an existing image hosted in a repository\. 

The following snippet is an example of the `Dockerfile`\. When you follow the instructions in [Using the Docker platform](single-container-docker.md), you can upload this `Dockerfile` as written\. Elastic Beanstalk runs the game 2048 when you use this `Dockerfile`\.

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