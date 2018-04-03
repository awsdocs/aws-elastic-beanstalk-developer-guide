# Configuring Docker Environments<a name="create_deploy_docker.container.console"></a>

You can use the Elastic Beanstalk console to configure the software running on your environment's EC2 instances\.

**To access the software configuration for your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

The Log Options section has two settings:
+ **Instance profile** – Your environment's [instance profile](concepts-roles-instance.md), which must have write access to your environment's Amazon S3 storage bucket in order to upload logs\.
+ **Enable log file rotation to Amazon S3** – Configure the instances in your environment to [upload rotated logs](using-features.logging.md)\.

The **Environment Properties** section lets you specify environment variables that you can read from your application code\.

**Topics**
+ [Docker Images](#docker-images)
+ [Configuring Additional Storage Volumes](#docker-volumes)
+ [Reclaiming Docker Storage Space](#reclaim-docker-storage)
+ [Configuring Managed Updates for Docker Environments](#docker-managed-updates)

## Docker Images<a name="docker-images"></a>

The single container and multicontainer Docker configuration for Elastic Beanstalk support the use of Docker images stored in a public or private online image repository\.

Specify images by name in `Dockerrun.aws.json`\. Note these conventions:
+ Images in official repositories on Docker Hub use a single name \(for example, `ubuntu` or `mongo`\)\.
+ Images in other repositories on Docker Hub are qualified with an organization name \(for example, `amazon/amazon-ecs-agent`\)\.
+ Images in other online repositories are qualified further by a domain name \(for example, `quay.io/assemblyline/ubuntu` or `account-id.dkr.ecr.us-east-2.amazonaws.com/ubuntu:trusty`\)\. 

For single container environments only, you can also build your own image during environment creation with a Dockerfile\. See [Building Custom Images with a Dockerfile](create_deploy_docker_image.md#create_deploy_docker_image_dockerfile) for details\.

### Using Images from an Amazon ECR Repository<a name="docker-images-ecr"></a>

You can store your custom Docker images in AWS with [Amazon Elastic Container Registry](https://aws.amazon.com/ecr) \(Amazon ECR\)\. When you store your Docker images in Amazon ECR, Elastic Beanstalk automatically authenticates to the Amazon ECR registry with your environment's [instance profile](concepts-roles-instance.md), so you don't need to [generate an authentication file](#docker-images-private) and upload it to Amazon Simple Storage Service \(Amazon S3\)\.

You do, however, need to provide your instances with permission to access the images in your Amazon ECR repository by adding permissions to your environment's instance profile\. You can attach the [AmazonEC2ContainerRegistryReadOnly](http://docs.aws.amazon.com/AmazonECR/latest/userguide/ecr_managed_policies.html#AmazonEC2ContainerRegistryReadOnly) managed policy to the instance profile to provide read\-only access to all Amazon ECR repositories in your account, or grant access to single repository by using the following template to create a custom policy:

```
{
  "Version": "2008-10-17",
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

In your `Dockerrun.aws.json` file, refer to the image by URL\. For a [single container configuration](create_deploy_docker_image.md), the URL goes in the `Image` definition:

```
  "Image": {
    "Name": "account-id.dkr.ecr.us-east-2.amazonaws.com/repository-name:latest",
    "Update": "true"
  },
```

For a [multicontainer configuration](create_deploy_docker_v2config.md), use the `image` key in a container definition object:

```
"containerDefinitions": [
      {
      "name": "my-image",
      "image": "account-id.dkr.ecr.us-east-2.amazonaws.com/repository-name:latest",
```

### Using Images From a Private Repository<a name="docker-images-private"></a>

To use a Docker image in a private repository hosted by an online registry, you must provide an authentication file that contains information required to authenticate with the registry\.

Generate an authentication file with the docker login command\. For repositories on Docker Hub, run `docker login`:

```
$ docker login
```

For other registries, include the URL of the registry server:

```
$ docker login registry-server-url
```

**Important**  
Beginning with Docker version 1\.7, the docker login command changed the name of the authentication file, and the format of the file\. Elastic Beanstalk requires the older `~/.dockercfg` format configuration file\.  
With Docker version 1\.7 and later, the docker login command creates the authentication file in `~/.docker/config.json` in the following format:  

```
{
  "auths" :
  {
    "server" :
    {
      "auth" : "auth_token",
      "email" : "email"
    }
  }
}
```
With Docker version 1\.6\.2 and earlier, the docker login command creates the authentication file in `~/.dockercfg` in the following format:  

```
{
  "server" :
  {
    "auth" : "auth_token",
    "email" : "email"
  }
}
```
To convert a `config.json` file, remove the outer `auths` key and flatten the JSON document to match the old format\.

Upload the authentication file to a secure Amazon S3 bucket\. The Amazon S3 bucket must be hosted in the same region as the environment that is using it\. Elastic Beanstalk cannot download files from an Amazon S3 bucket hosted in other regions\. Grant permissions for the `s3:GetObject` operation to the IAM role in the instance profile\. For details, see [Managing Elastic Beanstalk Instance Profiles](iam-instanceprofile.md)\.

Include the Amazon S3 bucket information in the `Authentication` \(v1\) or `authentication` \(v2\) parameter in your `Dockerrun.aws.json` file\.

For more information about the `Dockerrun.aws.json` format for single container environments, see [Single Container Docker Configuration](create_deploy_docker_image.md)\. For multicontainer environments, see [Multicontainer Docker Configuration](create_deploy_docker_v2config.md)\.

For more information about the authentication file, see [ Store images on Docker Hub ](https://docs.docker.com/docker-hub/repos/) and [ docker login ](https://docs.docker.com/engine/reference/commandline/login/) on the Docker website\.

## Configuring Additional Storage Volumes<a name="docker-volumes"></a>

For improved performance, Elastic Beanstalk configures two Amazon EBS storage volumes for your Docker environment's EC2 instances\. In addition to the root volume provisioned for all Elastic Beanstalk environments, a second 12GB volume named `xvdcz` is provisioned for image storage on Docker environments\.

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

Note that when you change settings in this namespace, Elastic Beanstalk replaces all instances in your environment with instances running the new configuration\. See [Configuration Changes](environments-updating.md) for details\.

## Reclaiming Docker Storage Space<a name="reclaim-docker-storage"></a>

Docker does not clean up \(delete\) the space used when a file is created and then deleted from within a running container; the space is only returned to the pool once the container is deleted\. This becomes an issue if a container process creates and deletes many files, such as regularly dumping database backups, filling up the application storage space\.

One solution is to increase the size of the application storage space, as described in the previous section\. The other option is less\-performant: run `fstrim` on the host OS periodically, such as using `cron`, against container free space to reclaim the unused container data blocks\.

```
docker ps -q | xargs docker inspect --format='{{ .State.Pid }}' | xargs -IZ sudo fstrim /proc/Z/root/
```

## Configuring Managed Updates for Docker Environments<a name="docker-managed-updates"></a>

With [managed platform updates](environment-platform-update-managed.md), you can configure your environment to automatically upgrade to the latest version of a platform on a schedule\.

In the case of Docker environments, you might want to decide if an automatic platform update should happen across Docker versions—when the new platform configuration version includes a new Docker version\. Elastic Beanstalk supports managed platform updates across Docker versions when updating from an environment running a Docker configuration version newer than 2\.9\.0\. When a new platform configuration version includes a new version of Docker, Elastic Beanstalk increments the minor update version number\. Therefore, to allow managed platform updates across Docker versions, enable managed platform updates for both minor and patch version updates\. To prevent managed platform updates across Docker versions, enable managed platform updates to apply patch version updates only\.

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

For environments running Docker configuration versions 2\.9\.0 or earlier, Elastic Beanstalk never performs managed platform updates if the new platform configuration version includes a new Docker version\.