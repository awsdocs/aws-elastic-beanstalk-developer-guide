# Multicontainer Docker environments<a name="create_deploy_docker_ecs"></a>

 You can create docker environments that support multiple containers per Amazon EC2 instance with multicontainer Docker platform for Elastic Beanstalk\. 

 Elastic Beanstalk uses Amazon Elastic Container Service \(Amazon ECS\) to coordinate container deployments to multicontainer Docker environments\. Amazon ECS provides tools to manage a cluster of instances running Docker containers\. Elastic Beanstalk takes care of Amazon ECS tasks including cluster creation, task definition and execution\. 

**Note**  
Some regions don't offer Amazon ECS\. Multicontainer Docker environments aren't supported in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

**Topics**
+ [Multicontainer Docker platform](#create_deploy_docker_ecs_platform)
+ [Dockerrun\.aws\.json file](#create_deploy_docker_ecs_dockerrun)
+ [Docker images](#create_deploy_docker_ecs_images)
+ [Container instance role](#create_deploy_docker_ecs_role)
+ [Amazon ECS resources created by Elastic Beanstalk](#create_deploy_docker_ecs_resources)
+ [Using multiple Elastic Load Balancing listeners](#create_deploy_docker_ecs_listeners)
+ [Failed container deployments](#create_deploy_docker_ecs_rollback)
+ [Multicontainer Docker configuration](create_deploy_docker_v2config.md)
+ [Multicontainer Docker environments with the Elastic Beanstalk console](create_deploy_docker_ecstutorial.md)

## Multicontainer Docker platform<a name="create_deploy_docker_ecs_platform"></a>

 Standard generic and preconfigured Docker platforms on Elastic Beanstalk support only a single Docker container per Elastic Beanstalk environment\. In order to get the most out of Docker, Elastic Beanstalk lets you create an environment where your Amazon EC2 instances run multiple Docker containers side by side\.

The following diagram shows an example Elastic Beanstalk environment configured with three Docker containers running on each Amazon EC2 instance in an Auto Scaling group:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-multicontainer-docker-example.png)

## Dockerrun\.aws\.json file<a name="create_deploy_docker_ecs_dockerrun"></a>

 Container instances—Amazon EC2 instances running Multicontainer Docker in an Elastic Beanstalk environment—require a configuration file named `Dockerrun.aws.json`\. This file is specific to Elastic Beanstalk and can be used alone or combined with source code and content in a [source bundle](applications-sourcebundle.md) to create an environment on a Docker platform\. 

**Note**  
 Version 1 of the `Dockerrun.aws.json` format is used to launch a single Docker container to an Elastic Beanstalk environment\. Version 2 adds support for multiple containers per Amazon EC2 instance and can only be used with the multicontainer Docker platform\. The format differs significantly from the previous version which is detailed under [Docker configuration](single-container-docker-configuration.md) 

 See [Dockerrun\.aws\.json v2](create_deploy_docker_v2config.md#create_deploy_docker_v2config_dockerrun) for details on the updated format and an example file\. 

## Docker images<a name="create_deploy_docker_ecs_images"></a>

 The Multicontainer Docker platform for Elastic Beanstalk requires images to be prebuilt and stored in a public or private online image repository\. 

**Note**  
 Building custom images during deployment with a `Dockerfile` is not supported by the multicontainer Docker platform on Elastic Beanstalk\. Build your images and deploy them to an online repository before creating an Elastic Beanstalk environment\. 

 Specify images by name in `Dockerrun.aws.json`\. Note these conventions:
+  Images in official repositories on Docker Hub use a single name \(for example, `ubuntu` or `mongo`\)\.
+ Images in other repositories on Docker Hub are qualified with an organization name \(for example, `amazon/amazon-ecs-agent`\)\.
+ Images in other online registries are qualified further by a domain name \(for example, `quay.io/assemblyline/ubuntu`\)\. 

 To configure Elastic Beanstalk to authenticate to a private repository, include the `authentication` parameter in your `Dockerrun.aws.json` file\. 

## Container instance role<a name="create_deploy_docker_ecs_role"></a>

 Elastic Beanstalk uses an Amazon ECS\-optimized AMI with an Amazon ECS container agent that runs in a Docker container\. The agent communicates with Amazon ECS to coordinate container deployments\. In order to communicate with Amazon ECS, each Amazon EC2 instance must have the corresponding permissions in IAM\. These permissions are attached to the default [instance profile](concepts-roles.md) when you create an environment in the Elastic Beanstalk Management Console:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ECSAccess",
      "Effect": "Allow",
      "Action": [
        "ecs:Poll",
        "ecs:StartTask",
        "ecs:StopTask",
        "ecs:DiscoverPollEndpoint",
        "ecs:StartTelemetrySession",
        "ecs:RegisterContainerInstance",
        "ecs:DeregisterContainerInstance",
        "ecs:DescribeContainerInstances",
        "ecs:Submit*"
      ],
      "Resource": "*"
    }
  ]
}
```

If you create your own instance profile, you can attach the `AWSElasticBeanstalkMulticontainerDocker` managed policy to make sure the permissions stay up\-to\-date\. For instructions on creating policies and roles in IAM, see [ Creating IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-creatingrole.html) in the IAM User Guide\. 

## Amazon ECS resources created by Elastic Beanstalk<a name="create_deploy_docker_ecs_resources"></a>

 When you create an environment using the multicontainer Docker platform, Elastic Beanstalk automatically creates and configures several Amazon Elastic Container Service resources while building the environment in order to create the necessary containers on each Amazon EC2 instance\. 
+ **Amazon ECS Cluster** – Container instances in Amazon ECS are organized into clusters\. When used with Elastic Beanstalk, one cluster is always created for each multicontainer Docker environment\. 
+ **Amazon ECS Task Definition** – Elastic Beanstalk uses the `Dockerrun.aws.json` file in your project to generate the Amazon ECS task definition that is used to configure container instances in the environment\. 
+ **Amazon ECS Task** – Elastic Beanstalk communicates with Amazon ECS to run a task on every instance in the environment to coordinate container deployment\. In a scalable environment, Elastic Beanstalk initiates a new task whenever an instance is added to the cluster\. In rare cases you may have to increase the amount of space reserved for containers and images\. Learn more in the [Configuring Docker environments](create_deploy_docker.container.console.md) section\.
+ **Amazon ECS Container Agent** – The agent runs in a Docker container on the instances in your environment\. The agent polls the Amazon ECS service and waits for a task to run\. 
+ **Amazon ECS Data Volumes** – Elastic Beanstalk inserts volume definitions \(in addition to the volumes that you define in `Dockerrun.aws.json`\) into the task definition to facilitate log collection\. 

   Elastic Beanstalk creates log volumes on the container instance, one for each container, at `/var/log/containers/containername`\. These volumes are named `awseb-logs-containername` and are provided for containers to mount\. See [Container definition format](create_deploy_docker_v2config.md#create_deploy_docker_v2config_dockerrun_format) for details on how to mount them\. 

## Using multiple Elastic Load Balancing listeners<a name="create_deploy_docker_ecs_listeners"></a>

You can configure multiple Elastic Load Balancing listeners on a multicontainer Docker environment in order to support inbound traffic for proxies or other services that don't run on the default HTTP port\.

Create a `.ebextensions` folder in your source bundle and add a file with a `.config` file extension\. The following example shows a configuration file that creates an Elastic Load Balancing listener on port 8080\.

**`.ebextensions/elb-listener.config`**

```
option_settings:
  aws:elb:listener:8080:
    ListenerProtocol: HTTP
    InstanceProtocol: HTTP
    InstancePort: 8080
```

If your environment is running in a custom [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) that you created, Elastic Beanstalk takes care of the rest\. In a default VPC, you need to configure your instance's security group to allow ingress from the load balancer\. Add a second configuration file that adds an ingress rule to the security group:

**`.ebextensions/elb-ingress.config`**

```
Resources:
  port8080SecurityGroupIngress:
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
      IpProtocol: tcp
      ToPort: 8080
      FromPort: 8080
      SourceSecurityGroupName: { "Fn::GetAtt": ["AWSEBLoadBalancer", "SourceSecurityGroup.GroupName"] }
```

For more information on the configuration file format, see [Adding and customizing Elastic Beanstalk environment resources](environment-resources.md) and [Option settings](ebextensions-optionsettings.md)\. 

 In addition to adding a listener to the Elastic Load Balancing configuration and opening a port in the security group, you need to map the port on the host instance to a port on the Docker container in the `containerDefinitions` section of the `Dockerrun.aws.json` file\. The following excerpt shows an example: 

```
"portMappings": [
  {
    "hostPort": 8080,
    "containerPort": 8080
  }
]
```

See [Dockerrun\.aws\.json v2](create_deploy_docker_v2config.md#create_deploy_docker_v2config_dockerrun) for details on the `Dockerrun.aws.json` file format\. 

## Failed container deployments<a name="create_deploy_docker_ecs_rollback"></a>

 If an Amazon ECS task fails, one or more containers in your Elastic Beanstalk environment will not start\. Elastic Beanstalk does not roll back multicontainer environments due to a failed Amazon ECS task\. If a container fails to start in your environment, redeploy the current version or a previous working version from the Elastic Beanstalk console\. 

**To deploy an existing version**

1. Open the Elastic Beanstalk console in your environment's region\.

1. Click **Actions** to the right of your application name and then click **View application versions**\.

1. Select a version of your application and click **Deploy**\.