# Configuring Docker environments<a name="create_deploy_docker.container.console"></a>

There are several ways to configure the behavior of your Elastic Beanstalk Docker environment\.

**Note**  
If your Elastic Beanstalk environment uses an Amazon Linux AMI Docker platform version \(preceding Amazon Linux 2\), be sure to read the additional information in [Docker configuration on Amazon Linux AMI \(preceding Amazon Linux 2\)](#docker-alami)\.

**Topics**
+ [Configuring software in Docker environments](#docker-software-config)
+ [Docker images](#docker-images)
+ [Reclaiming Docker storage space](#reclaim-docker-storage)
+ [Configuring managed updates for Docker environments](#docker-managed-updates)
+ [Docker configuration on Amazon Linux AMI \(preceding Amazon Linux 2\)](#docker-alami)
+ [Docker configuration namespaces](#docker-namespaces)

## Configuring software in Docker environments<a name="docker-software-config"></a>

You can use the Elastic Beanstalk console to configure the software running on your environment's instances\.

**To configure your Docker environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. Make necessary configuration changes\.

1. Choose **Apply**\.

For information about configuring software settings in any environment, see [Environment properties and other software settings](environments-cfg-softwaresettings.md)\. The following sections cover Docker specific information\.

### Container options<a name="docker-software-config.container"></a>

The **Container options** section has platform\-specific options\. For Docker environments, it lets you choose whether or not your environment includes the Nginx proxy server\.

### Environment properties<a name="docker-software-config.env"></a>

The **Environment properties** section lets you specify environment configuration settings on the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

In a Docker environment, Elastic Beanstalk passes environment properties to Docker containers as environment variables\. Your application code running in a container can refer to an environment variable by name and read its value\. The code to do so depends on the programming language you're using\. You can find instructions for reading environment variable values in the programming languages that Elastic Beanstalk managed platforms support in the respective platform topic\. For a list of links to these topics, see [Environment properties and other software settings](environments-cfg-softwaresettings.md)\.

## Docker images<a name="docker-images"></a>

The Docker and Multicontainer Docker platforms for Elastic Beanstalk support the use of Docker images stored in a public or private online image repository\.

Specify images by name in `Dockerrun.aws.json`\. Note these conventions:
+ Images in official repositories on Docker Hub use a single name \(for example, `ubuntu` or `mongo`\)\.
+ Images in other repositories on Docker Hub are qualified with an organization name \(for example, `amazon/amazon-ecs-agent`\)\.
+ Images in other online repositories are qualified further by a domain name \(for example, `quay.io/assemblyline/ubuntu` or `account-id.dkr.ecr.us-east-2.amazonaws.com/ubuntu:trusty`\)\. 

For environments using the Docker platform only, you can also build your own image during environment creation with a Dockerfile\. See [Building custom images with a Dockerfile](single-container-docker-configuration.md#single-container-docker-configuration.dockerfile) for details\. The Multicontainer Docker platform doesn't support this functionality\.

### Using images from an Amazon ECR repository<a name="docker-images-ecr"></a>

You can store your custom Docker images in AWS with [Amazon Elastic Container Registry](https://aws.amazon.com/ecr) \(Amazon ECR\)\. When you store your Docker images in Amazon ECR, Elastic Beanstalk automatically authenticates to the Amazon ECR registry with your environment's [instance profile](concepts-roles-instance.md), so you don't need to [generate an authentication file](#docker-images-private) and upload it to Amazon Simple Storage Service \(Amazon S3\)\.

You do, however, need to provide your instances with permission to access the images in your Amazon ECR repository by adding permissions to your environment's instance profile\. You can attach the [AmazonEC2ContainerRegistryReadOnly](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ecr_managed_policies.html#AmazonEC2ContainerRegistryReadOnly) managed policy to the instance profile to provide read\-only access to all Amazon ECR repositories in your account, or grant access to single repository by using the following template to create a custom policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "AllowEbAuth",
        "Effect": "Allow",
        "Action": [
          "ecr:GetAuthorizationToken"
        ],
        "Resource": [
          "*"
        ]
      },
      {
        "Sid": "AllowPull",
        "Effect": "Allow",
        "Resource": [
          "arn:aws:ecr:us-east-2:account-id:repository/repository-name"
        ],
        "Action": [
          "ecr:GetAuthorizationToken",
          "ecr:BatchCheckLayerAvailability",
          "ecr:GetDownloadUrlForLayer",
          "ecr:GetRepositoryPolicy",
          "ecr:DescribeRepositories",
          "ecr:ListImages",
          "ecr:BatchGetImage"
        ]
      }
    ]
  }
```

Replace the Amazon Resource Name \(ARN\) in the above policy with the ARN of your repository\.

In your `Dockerrun.aws.json` file, refer to the image by URL\. For the [Docker platform](single-container-docker-configuration.md), the URL goes in the `Image` definition:

```
  "Image": {
      "Name": "account-id.dkr.ecr.us-east-2.amazonaws.com/repository-name:latest",
      "Update": "true"
    },
```

For the [Multicontainer Docker platform](create_deploy_docker_v2config.md), use the `image` key in a container definition object:

```
"containerDefinitions": [
        {
        "name": "my-image",
        "image": "account-id.dkr.ecr.us-east-2.amazonaws.com/repository-name:latest",
```

### Using images from a private repository<a name="docker-images-private"></a>

To use a Docker image in a private repository hosted by an online registry, you must provide an authentication file that contains information required to authenticate with the registry\.

Generate an authentication file with the docker login command\. For repositories on Docker Hub, run `docker login`:

```
$ docker login
```

For other registries, include the URL of the registry server:

```
$ docker login registry-server-url
```

**Note**  
If your Elastic Beanstalk environment uses an Amazon Linux AMI Docker platform version \(preceding Amazon Linux 2\), read the additional information in [Docker configuration on Amazon Linux AMI \(preceding Amazon Linux 2\)](#docker-alami)\.

Upload a copy named `.dockercfg` of the authentication file to a secure Amazon S3 bucket\. The Amazon S3 bucket must be hosted in the same AWS Region as the environment that is using it\. Elastic Beanstalk cannot download files from an Amazon S3 bucket hosted in other Regions\. Grant permissions for the `s3:GetObject` operation to the IAM role in the instance profile\. For details, see [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)\.

Include the Amazon S3 bucket information in the `Authentication` \(v1\) or `authentication` \(v2\) parameter in your `Dockerrun.aws.json` file\.

For more information about the `Dockerrun.aws.json` format for Docker environments, see [Docker configuration](single-container-docker-configuration.md)\. For multicontainer environments, see [Multicontainer Docker configuration](create_deploy_docker_v2config.md)\.

For more information about the authentication file, see [ Store images on Docker Hub ](https://docs.docker.com/docker-hub/repos/) and [ docker login ](https://docs.docker.com/engine/reference/commandline/login/) on the Docker website\.

## Reclaiming Docker storage space<a name="reclaim-docker-storage"></a>

Docker does not clean up \(delete\) the space used when a file is created and then deleted from within a running container; the space is only returned to the pool once the container is deleted\. This becomes an issue if a container process creates and deletes many files, such as regularly dumping database backups, filling up the application storage space\.

One solution is to increase the size of the application storage space, as described in the previous section\. The other option is less\-performant: run `fstrim` on the host OS periodically, such as using `cron`, against container free space to reclaim the unused container data blocks\.

```
docker ps -q | xargs docker inspect --format='{{ .State.Pid }}' | xargs -IZ sudo fstrim /proc/Z/root/
```

## Configuring managed updates for Docker environments<a name="docker-managed-updates"></a>

With [managed platform updates](environment-platform-update-managed.md), you can configure your environment to automatically update to the latest version of a platform on a schedule\.

In the case of Docker environments, you might want to decide if an automatic platform update should happen across Docker versions—when the new platform version includes a new Docker version\. Elastic Beanstalk supports managed platform updates across Docker versions when updating from an environment running a Docker platform version newer than 2\.9\.0\. When a new platform version includes a new version of Docker, Elastic Beanstalk increments the minor update version number\. Therefore, to allow managed platform updates across Docker versions, enable managed platform updates for both minor and patch version updates\. To prevent managed platform updates across Docker versions, enable managed platform updates to apply patch version updates only\.

For example, the following [configuration file](ebextensions.md) enables managed platform updates at 9:00 AM UTC each Tuesday for both minor and patch version updates, thereby allowing for managed updates across Docker versions:

**Example \.ebextensions/managed\-platform\-update\.config**  

```
option_settings:
  aws:elasticbeanstalk:managedactions:
    ManagedActionsEnabled: true
    PreferredStartTime: "Tue:09:00"
  aws:elasticbeanstalk:managedactions:platformupdate:
    UpdateLevel: minor
```

For environments running Docker platform versions 2\.9\.0 or earlier, Elastic Beanstalk never performs managed platform updates if the new platform version includes a new Docker version\.

## Docker configuration on Amazon Linux AMI \(preceding Amazon Linux 2\)<a name="docker-alami"></a>

If your Elastic Beanstalk Docker environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Using an authentication file for a private repository<a name="docker-alami.images-private"></a>

This information is relevant to you if you are [using images from a private repository](#docker-images-private)\. Beginning with Docker version 1\.7, the docker login command changed the name of the authentication file, and the format of the file\. Amazon Linux AMI Docker platform versions \(preceding Amazon Linux 2\) require the older `~/.dockercfg` format configuration file\.

With Docker version 1\.7 and later, the docker login command creates the authentication file in `~/.docker/config.json` in the following format\.

```
{
    "auths":{
      "server":{
        "auth":"key"
      }
    }
  }
```

With Docker version 1\.6\.2 and earlier, the docker login command creates the authentication file in `~/.dockercfg` in the following format\.

```
{
    "server" :
    {
      "auth" : "auth_token",
      "email" : "email"
    }
  }
```

To convert a `config.json` file, remove the outer `auths` key, add an `email` key, and flatten the JSON document to match the old format\.

On Amazon Linux 2 Docker platform versions, Elastic Beanstalk uses the newer authentication file name and format\. If you're using an Amazon Linux 2 Docker platform version, you can use the authentication file that the docker login command creates without any conversion\.

### Configuring additional storage volumes<a name="docker-alami.volumes"></a>

For improved performance on Amazon Linux AMI, Elastic Beanstalk configures two Amazon EBS storage volumes for your Docker environment's Amazon EC2 instances\. In addition to the root volume provisioned for all Elastic Beanstalk environments, a second 12GB volume named `xvdcz` is provisioned for image storage on Docker environments\.

If you need more storage space or increased IOPS for Docker images, you can customize the image storage volume by using the `BlockDeviceMapping` configuration option in the [aws:autoscaling:launchconfiguration](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) namespace\.

For example, the following [configuration file](ebextensions.md) increases the storage volume's size to 100 GB with 500 provisioned IOPS:

**Example \.ebextensions/blockdevice\-xvdcz\.config**  

```
option_settings:
  aws:autoscaling:launchconfiguration:
    BlockDeviceMappings: /dev/xvdcz=:100::io1:500
```

If you use the `BlockDeviceMappings` option to configure additional volumes for your application, you should include a mapping for `xvdcz` to ensure that it is created\. The following example configures two volumes, the image storage volume `xvdcz` with default settings and an additional 24 GB application volume named `sdh`:

**Example \.ebextensions/blockdevice\-sdh\.config**  

```
option_settings:
  aws:autoscaling:launchconfiguration:
    BlockDeviceMappings: /dev/xvdcz=:12:true:gp2,/dev/sdh=:24
```

**Note**  
When you change settings in this namespace, Elastic Beanstalk replaces all instances in your environment with instances running the new configuration\. See [Configuration changes](environments-updating.md) for details\.

## Docker configuration namespaces<a name="docker-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Docker platform supports options in the following namespaces, in addition to the [options supported for all Elastic Beanstalk environments](command-options-general.md):
+ `aws:elasticbeanstalk:environment:proxy` – Choose the proxy server for your environment\. Docker supports either running Nginx or no proxy server\.

The following example configuration file configures a Docker environment to run no proxy server\.

**Example \.ebextensions/docker\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: none
```