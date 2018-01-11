# Single Container Docker Configuration<a name="create_deploy_docker_image"></a>

This section describes how to prepare your Docker image and container for uploading to Elastic Beanstalk\. Any web application that you deploy to Elastic Beanstalk in single\-container Docker container must include a `Dockerfile`, which defines a custom image, a `Dockerrun.aws.json` file, which specifies an existing image to use and environment configuration, or both\. You can deploy your web application from a Docker container to Elastic Beanstalk by doing one of the following:

+ Create a `Dockerfile` to customize an image and to deploy a Docker container to Elastic Beanstalk\.

+ Create a `Dockerrun.aws.json` file to deploy a Docker container from an existing Docker image to Elastic Beanstalk\.

+ Create a `.zip` file containing your application files, any application file dependencies, the `Dockerfile`, and the `Dockerrun.aws.json` file\.

  If you use only a `Dockerfile` or only a `Dockerrun.aws.json` file to deploy your application, you do not need to compress the file into a \.zip file\.


+ [Dockerrun\.aws\.json v1](#create_deploy_docker_image_dockerrun)
+ [Using Images from a Private Repository](#docker-singlecontainer-dockerrun-privaterepo)
+ [Building Custom Images with a Dockerfile](#create_deploy_docker_image_dockerfile)

## Dockerrun\.aws\.json v1<a name="create_deploy_docker_image_dockerrun"></a>

A `Dockerrun.aws.json` file describes how to deploy a Docker container as an Elastic Beanstalk application\. This JSON file is specific to Elastic Beanstalk\. If your application runs on an image that is available in a hosted repository, you can specify the image in a `Dockerrun.aws.json` file and omit the `Dockerfile`\.

Valid keys and values for the `Dockerrun.aws.json` file include the following:

**AWSEBDockerrunVersion**  
\(Required\) Specifies the version number as the value `1` for single container Docker environments\.

**Authentication**  
\(Required only for private repositories\) Specifies the Amazon S3 object storing the `.dockercfg` file\.  
See \.

**Image**  
Specifies the Docker base image on an existing Docker repository from which you're building a Docker container\. Specify the value of the **Name** key in the format *<organization>/<image name>* for images on Docker Hub, or *<site>/<organization name>/<image name>* for other sites\.   
When you specify an image in the `Dockerrun.aws.json` file, each instance in your Elastic Beanstalk environment will run `docker pull` on that image and run it\. Optionally include the **Update** key\. The default value is "true" and instructs Elastic Beanstalk to check the repository, pull any updates to the image, and overwrite any cached images\.  
Do not specify the **Image** key in the `Dockerrun.aws.json` file when using a `Dockerfile`\. \.Elastic Beanstalk always builds and uses the image described in the `Dockerfile` when one is present\.

**Ports**  
\(Required when you specify the **Image** key\) Lists the ports to expose on the Docker container\. Elastic Beanstalk uses **ContainerPort** value to connect the Docker container to the reverse proxy running on the host\.  
You can specify multiple container ports, but Elastic Beanstalk uses only the first one to connect your container to the host's reverse proxy and route requests from the public Internet\.

****Volumes****  
Map volumes from an EC2 instance to your Docker container\. Specify one or more arrays of volumes to map\.

****Logging****  
Specify the directory to which your application writes logs\. Elastic Beanstalk uploads any logs in this directory to Amazon S3 when you request tail or bundle logs\. If you rotate logs to a folder named `rotated` within this directory, you can also configure Elastic Beanstalk to upload rotated logs to Amazon S3 for permanent storage\. For more information, see [Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances](using-features.logging.md)\.

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
  "Logging": "/var/log/nginx"
}
```

You can provide Elastic Beanstalk with only the `Dockerrun.aws.json` file, or with a `.zip` archive containing both the `Dockerrun.aws.json` and `Dockerfile` files\. When you provide both files, the `Dockerfile` describes the Docker image and the `Dockerrun.aws.json` file provides additional information for deployment as described later in this section\.

**Note**  
The two files must be at the root, or top level, of the `.zip` archive\. Do not build the archive from a directory containing the files\. Navigate into that directory and build the archive there\.

**Note**  
When you provide both files, do not specify an image in the `Dockerrun.aws.json` file\. Elastic Beanstalk builds and uses the image described in the `Dockerfile` and ignores the image specified in the `Dockerrun.aws.json` file\.

## Using Images from a Private Repository<a name="docker-singlecontainer-dockerrun-privaterepo"></a>

Add the information about the Amazon S3 bucket that contains the authentication file in the `Authentication` parameter of the `Dockerrun.aws.json` file\. Make sure that the `Authentication` parameter contains a valid Amazon S3 bucket and key\. The Amazon S3 bucket must be hosted in the same region as the environment that is using it\. Elastic Beanstalk will not download files from Amazon S3 buckets hosted in other regions\. 

For information about generating and uploading the authentication file, see [Using Images From a Private Repository](create_deploy_docker.container.console.md#docker-images-private)\.

The following example shows the use of an authentication file named `mydockercfg` in a bucket named `my-bucket` to use a private image in a third party registry\.

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

## Building Custom Images with a Dockerfile<a name="create_deploy_docker_image_dockerfile"></a>

Docker uses a `Dockerfile` to create a Docker image that contains your source bundle\. A Docker image is the template from which you create a Docker container\. `Dockerfile` is a plain text file that contains instructions that Elastic Beanstalk uses to build a customized Docker image on each Amazon EC2 instance in your Elastic Beanstalk environment\. Create a `Dockerfile` when you do not already have an existing image hosted in a repository\. 

Include the following instructions in the `Dockerfile`:

**FROM**  
\(Required as the first instruction in the file\) Specifies the base image from which to build the Docker container and against which Elastic Beanstalk runs subsequent `Dockerfile` instructions\.  
The image can be hosted in a public repository, a private repository hosted by a third\-party registry, or a repository that you run on EC2\.

**EXPOSE**  
\(Required\) Lists the ports to expose on the Docker container\. Elastic Beanstalk uses the port value to connect the Docker container to the reverse proxy running on the host\.  
You can specify multiple container ports, but Elastic Beanstalk uses only the first one to connect your container to the host's reverse proxy and route requests from the public Internet\.

**CMD**  
Specifies an executable and default parameters, which are combined into the command that the container runs at launch\. Use the following format:  

```
CMD ["executable","param1","param2"]
```
`CMD` can also be used to provide default parameters for an `ENTRYPOINT` command by omitting the executable argument\. An executable must be specified in either a `CMD` or an `ENTRYPOINT`, but not both\. For basic scenarios, use a `CMD` and omit the `ENTRYPOINT`\. 

**ENTRYPOINT**  
Uses the same JSON format as `CMD` and, like `CMD`, specifies a command to run when the container is launched\. Also allows a container to be run as an executable with `docker run`\.  
If you define an `ENTRYPOINT`, you can use a CMD as well to specify default parameters that can be overridden with `docker run`'s `-d` option\. The command defined by an `ENTRYPOINT` \(including any parameters\) is combined with parameters from `CMD` or `docker run` when the container is run\. 

**RUN**  
Specifies one or more commands that install packages and configure your web application inside the image\.  
If you include RUN instructions in the `Dockerfile`, compress the file and the context used by RUN instructions in the `Dockerfile` into a `.zip` file\. Compress files at the top level of the directory\.

The following snippet is an example of the `Dockerfile`\. When you follow the instructions in [Single Container Docker Environments](docker-singlecontainer-deploy.md), you can upload this `Dockerfile` as written\. Elastic Beanstalk runs the game 2048 when you use this `Dockerfile`\.

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

For more information about instructions you can include in the `Dockerfile`, go to [Dockerfile reference](https://docs.docker.com/engine/reference/builder) on the Docker website\.