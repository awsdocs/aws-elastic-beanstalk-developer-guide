# Preconfigured Docker containers<a name="create_deploy_dockerpreconfig"></a>

Elastic Beanstalk has a platform branch running a Docker container that is preconfigured with the Java EE GlassFish application server software stack\. You can use the preconfigured Docker container to develop and test your application locally and then deploy the application in an Elastic Beanstalk environment that is identical to your local environment\.

**Notes**  
Elastic Beanstalk also supports platform branches with preconfigured Docker containers for Go and Python\. These platform branches are scheduled for retirement\.
All the Preconfigured Docker platform branches use the Amazon Linux AMI operating system \(preceding Amazon Linux 2\)\. To migrate your GlassFish application to Amazon Linux 2, use the generic Docker platform and deploy GlassFish and your application code to an Amazon Linux 2 Docker image\. For details, see [Deploying a GlassFish application to the Docker platform](docker-glassfish-tutorial.md)\.

The following section provides a detailed procedure for deploying an application to Elastic Beanstalk using a preconfigured Docker container\.

For details about currently supported preconfigured Docker platform versions, see [Preconfigured Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.dockerpreconfig) in the *AWS Elastic Beanstalk Platforms* document\.

## Getting started with preconfigured Docker containers<a name="create_deploy_dockerpreconfig.walkthrough"></a>

This section shows you how to develop an example application locally and then deploy your application to Elastic Beanstalk with a preconfigured Docker container\.

### Set up your local development environment<a name="create_deploy_dockerpreconfig.walkthrough.setup"></a>

For this walk\-through we use a GlassFish example application\.

**To set up your environment**

1. Create a new folder for the example application\.

   ```
   ~$ mkdir eb-preconf-example
   ~$ cd eb-preconf-example
   ```

1. Download the example application code into the new folder\.

   ```
   ~$ wget https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/docker-glassfish-v1.zip
   ~$ unzip docker-glassfish-v1.zip
   ~$ rm docker-glassfish-v1.zip
   ```

### Develop and test locally<a name="create_deploy_dockerpreconfig.walkthrough.dev"></a>

**To develop an example GlassFish application**

1. Add a `Dockerfile` to your application’s root folder\. In the file, specify the AWS Elastic Beanstalk Docker base image to be used to run your local preconfigured Docker container\. You'll later deploy your application to an Elastic Beanstalk Preconfigured Docker GlassFish platform version\. Choose the Docker base image that this platform version uses\. To find out the current Docker image of the platform version, see the [Preconfigured Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.dockerpreconfig) section of the *AWS Elastic Beanstalk Supported Platforms* page in the *AWS Elastic Beanstalk Platforms* guide\.  
**Example \~/Eb\-preconf\-example/Dockerfile**  

   ```
   # For Glassfish 5.0 Java 8
   FROM amazon/aws-eb-glassfish:5.0-al-onbuild-2.11.1
   ```

   For more information about using a `Dockerfile`, see [Docker configuration](single-container-docker-configuration.md)\.

1. Build the Docker image\.

   ```
   ~/eb-preconf-example$ docker build -t my-app-image .
   ```

1. Run the Docker container from the image\.
**Note**  
You must include the `-p` flag to map port 8080 on the container to the localhost port 3000\. Elastic Beanstalk Docker containers always expose the application on port 8080 on the container\. The `-it` flags run the image as an interactive process\. The `--rm` flag cleans up the container file system when the container exits\. You can optionally include the `-d` flag to run the image as a daemon\.

   ```
   $ docker run -it --rm -p 3000:8080 my-app-image
   ```

1. To view the example application, type the following URL into your web browser\.

   ```
   http://localhost:3000
   ```

   The following web page appears\.  
![\[The GlassFish example application showing in a web browser\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dockerpreconfig-webpage.png)

### Deploy to Elastic Beanstalk<a name="create_deploy_dockerpreconfig.walkthrough.deploy"></a>

After testing your application, you are ready to deploy it to Elastic Beanstalk\.

**To deploy your application to Elastic Beanstalk**

1. In your application's root folder, rename the `Dockerfile` to `Dockerfile.local`\. This step is required for Elastic Beanstalk to use the `Dockerfile` that contains the correct instructions for Elastic Beanstalk to build a customized Docker image on each Amazon EC2 instance in your Elastic Beanstalk environment\.
**Note**  
You do not need to perform this step if your `Dockerfile` includes instructions that modify the platform version's base Docker image\. You do not need to use a `Dockerfile` at all if your `Dockerfile` includes only a `FROM` line to specify the base image from which to build the container\. In that situation, the `Dockerfile` is redundant\.

1. Create an application source bundle\.

   ```
   ~/eb-preconf-example$ zip myapp.zip -r *
   ```

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, under **Preconfigured – Docker**, choose **Glassfish**\.

1. For **Application code**, choose **Upload your code**, and then choose **Upload**\.

1. Choose **Local file**, choose **Browse**, and then open the application source bundle you just created\.

1. Choose **Upload**\.

1. Choose **Review and launch**\.

1. Review the available settings, and then choose **Create app**\.

1. When the environment is created, you can view the deployed application\. Choose the environment URL that is displayed at the top of the console dashboard\.