# Deploying a GlassFish application to the Docker platform<a name="docker-glassfish-tutorial"></a>

The goal of this tutorial is to provide customers using the Preconfigured Docker GlassFish platform \(based on Amazon Linux AMI\) with a migration path to Amazon Linux 2\.

The tutorial walks you through using the AWS Elastic Beanstalk Docker platform to deploy an application based on the [Java EE GlassFish application server](https://www.oracle.com/middleware/technologies/glassfish-server.html) to an Elastic Beanstalk environment\. 

We demonstrate two approaches to building a Docker image:
+ **Simple** – Provide your GlassFish application source code and let Elastic Beanstalk build and run a Docker image as part of provisioning your environment\. This is easy to set up, at a cost of increased instance provisioning time\.
+ **Advanced** – Build a custom Docker image containing your application code and dependencies, and provide it to Elastic Beanstalk to use in your environment\. This approach is slightly more involved, and decreases the provisioning time of instances in your environment\.

## Prerequisites<a name="docker-glassfish-tutorial.prereqs"></a>

This tutorial assumes that you have some knowledge of basic Elastic Beanstalk operations, [Using the Elastic Beanstalk command line interface \(EB CLI\)](eb-cli3.md), and Docker\. To follow this tutorial, you need a working local installation of Docker\. For more information about installing Docker, see the [Docker installation guide](https://docs.docker.com/install/)\.

If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\. This tutorial uses the EB CLI, but you can also create environments and upload applications by using the Elastic Beanstalk console\. To learn more about configuring Docker environments, see [Docker configuration](single-container-docker-configuration.md)\.

## Simple example: provide your application code<a name="docker-glassfish-tutorial.simple"></a>

This is an easy way to deploy your GlassFish application\. You provide your application source code together with the `Dockerfile` included in this tutorial\. Elastic Beanstalk builds a Docker image that includes your application and the GlassFish software stack\. Then Elastic Beanstalk runs the image on your environment instances\.

An issue with this approach is that Elastic Beanstalk builds the Docker image locally whenever it creates an instance for your environment\. The image build increases instance provisioning time\. This impact isn't limited to initial environment creation—it happens during scale\-out actions too\.

**To launch an environment with an example GlassFish application**

1. Download the example `docker-glassfish-al2-v1.zip`, and then expand the `.zip` file into a directory in your development environment\.

   ```
   ~$ curl https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/docker-glassfish-al2-v1.zip --output docker-glassfish-al2-v1.zip
   ~$ mkdir glassfish-example
   ~$ cd glassfish-example
   ~/glassfish-example$ unzip ../docker-glassfish-al2-v1.zip
   ```

   Your directory structure should be as follows\.

   ```
   ~/glassfish-example
   |-- Dockerfile
   |-- Dockerrun.aws.json
   |-- glassfish-start.sh
   |-- index.jsp
   |-- META-INF
   |   |-- LICENSE.txt
   |   |-- MANIFEST.MF
   |   `-- NOTICE.txt
   |-- robots.txt
   `-- WEB-INF
       `-- web.xml
   ```

   The following files are key to building and running a Docker container in your environment:
   + `Dockerfile` – Provides instructions that Docker uses to build an image with your application and required dependencies\.
   + `glassfish-start.sh` – A shell script that the Docker image runs to start your application\.
   + `Dockerrun.aws.json` – Provides a logging key, to include the GlassFish application server log in [log file requests](using-features.logging.md)\. If you aren't interested in GlassFish logs, you can omit this file\.

1. Configure your local directory for deployment to Elastic Beanstalk\.

   ```
   ~/glassfish-example$ eb init -p docker glassfish-example
   ```

1. \(Optional\) Use the eb local run command to build and run your container locally\.

   ```
   ~/glassfish-example$ eb local run --port 8080
   ```
**Note**  
To learn more about the eb local command, see [eb local](eb3-local.md)\. The command isn't supported on Windows\. Alternatively, you can build and run your container with the docker build and docker run commands\. For more information, see the [Docker documentation](https://docs.docker.com/)\.

1. \(Optional\) While your container is running, use the eb local open command to view your application in a web browser\. Alternatively, open [http://localhost:8080/](http://localhost:8080/) in a web browser\.

   ```
   ~/glassfish-example$ eb local open
   ```

1. Use the eb create command to create an environment and deploy your application\.

   ```
   ~/glassfish-example$ eb create glassfish-example-env
   ```

1. After your environment launches, use the eb open command to view it in a web browser\.

   ```
   ~/glassfish-example$ eb open
   ```

When you're done working with the example, terminate the environment and delete related resources\.

```
~/glassfish-example$ eb terminate --all
```

## Advanced example: provide a prebuilt Docker image<a name="docker-glassfish-tutorial.advanced"></a>

This is a more advanced way to deploy your GlassFish application\. Building on the first example, you create a Docker image containing your application code and the GlassFish software stack, and push it to Docker Hub\. After you've done this one\-time step, you can launch Elastic Beanstalk environments based on your custom image\.

When you launch an environment and provide your Docker image, instances in your environment download and use this image directly and don't need to build a Docker image\. Therefore, instance provisioning time is decreased\.

**Note**  
The following steps create a publicly available Docker image\.

**To launch an environment with a prebuilt GlassFish application Docker image**

1. Download and expand the example `docker-glassfish-al2-v1.zip` as in the previous [simple example](#docker-glassfish-tutorial.simple)\. If you've completed that example, you can use the directory you already have\.

1. Build a Docker image and push it to Docker Hub\.

   ```
   ~/glassfish-example$ docker build -t docker-username/beanstalk-glassfish-example:latest .
   ~/glassfish-example$ docker push docker-username/beanstalk-glassfish-example:latest
   ```
**Note**  
Before pushing your image, you might need to run docker login\.

1. Create an additional directory\.

   ```
   ~$ mkdir glassfish-prebuilt
   ~$ cd glassfish-prebuilt
   ```

1. Copy the following example into a file named `Dockerrun.aws.json`\.  
**Example `~/glassfish-prebuilt/Dockerrun.aws.json`**  

   ```
   {
     "AWSEBDockerrunVersion": "1",
     "Image": {
       "Name": "docker-username/beanstalk-glassfish-example"
     },
     "Ports": [
       {
         "ContainerPort": 8080,
         "HostPort": 8080
       }
     ],
     "Logging": "/usr/local/glassfish5/glassfish/domains/domain1/logs"
   }
   ```

1. Configure your local directory for deployment to Elastic Beanstalk\.

   ```
   ~/glassfish-prebuilt$ eb init -p docker glassfish-prebuilt$
   ```

1. \(Optional\) Use the eb local run command to run your container locally\.

   ```
   ~/glassfish-prebuilt$ eb local run --port 8080
   ```

1. \(Optional\) While your container is running, use the eb local open command to view your application in a web browser\. Alternatively, open [http://localhost:8080/](http://localhost:8080/) in a web browser\.

   ```
   ~/glassfish-prebuilt$ eb local open
   ```

1. Use the eb create command to create an environment and deploy your Docker image\.

   ```
   ~/glassfish-prebuilt$ eb create glassfish-prebuilt-env
   ```

1. After your environment launches, use the eb open command to view it in a web browser\.

   ```
   ~/glassfish-prebuilt$ eb open
   ```

When you're done working with the example, terminate the environment and delete related resources\.

```
~/glassfish-prebuilt$ eb terminate --all
```