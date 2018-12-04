# Using the Elastic Beanstalk Docker\-based Go Platform<a name="go_docker_platform"></a>

Follow the steps here to walk you through the process of deploying a Go application to Elastic Beanstalk with a preconfigured Docker container for Go\.

## Set Up Your Local Development Environment<a name="go_docker_platform.walkthrough.setup"></a>

This tutorial uses a Go “Hello World” application\.

**To set up your develop environment**

1. Create a new folder for the sample application\.

   ```
   ~$ mkdir eb-go-sample
   ~$ cd eb-go-sample
   ```

1. In the application's root folder, create a file with the name `server.go`\. In the file, include the following:  
**Example \~/eb\-go\-sample/server\.go**  

   ```
   package main
   
   import "github.com/go-martini/martini"
   
   func main() {
       m := martini.Classic()
       m.Get("/", func() string {
           return "Hello world!"
       })
       m.Run()
   }
   ```
**Note**  
The application source bundle must include a package called `main`\. Within that package, you must have a `main` function for the container to execute\.
Dependencies that need to be imported \(for example, the Martini package, `go-martini`\) will be downloaded to the container and installed during deployment\. Therefore, you do not need to include the dependencies in the application source bundle that you upload to Elastic Beanstalk\.
Elastic Beanstalk sets the container’s GOPATH environment variable to `/go`\.

## Develop and Test Locally Using Docker<a name="go_docker_platform.walkthrough.dev"></a>

With your environment set up, you're ready to create and test your Go application\.

**To develop a sample Go application**

1. Add a `Dockerfile` to your application’s root folder\. In the file, specify the Elastic Beanstalk Docker base image to use to run your local preconfigured Docker container\. Elastic Beanstalk uses this image to run any subsequent `Dockerfile` instructions\.
**Note**  
Include only the instruction with the Docker image name for your platform version\. For preconfigured Docker image names, see [Elastic Beanstalk Supported Platforms](concepts.platforms.md)\. For more information about using a `Dockerfile`, see [Single Container Docker Configuration](single-container-docker-configuration.md)\. For an example `Dockerfile` for preconfigured Docker platforms, see [Example: Using a Dockerfile to Customize and Configure a Preconfigured Docker Platform](create_deploy_dockerpreconfig.dockerfile.md)\.

   You can use the following example:  
**Example \~/eb\-go\-sample/Dockerfile**  

   ```
   # For Go 1.3
   FROM golang:1.3.3-onbuild
   
   # For Go 1.4
   FROM golang:1.4.1-onbuild
   ```

1. Build the Docker image\.

   ```
   ~/eb-go-sample$ docker build -t my-app-image .
   ```

1. Run the Docker container from the image\.
**Note**  
You must include the `-p` flag to map port 3000 on the container to the localhost port 8080\. Elastic Beanstalk preconfigured Docker containers for Go applications always expose the application on port 3000 on the container\. The `-it` flag runs the image as an interactive process\. The `-rm` flag cleans up the container file system when the container exits\. You can optionally include the `-d` flag to run the image as a daemon\.

   ```
   ~/eb-go-sample$ docker run -it --rm -p 8080:3000 my-app-image
   ```

1. To view the sample application, type the following URL into your web browser\.

   ```
   http://localhost:8080
   ```

## Deploy to Elastic Beanstalk<a name="go_docker_platform.walkthrough.deploy"></a>

After testing your application, you are ready to deploy it to Elastic Beanstalk\.

**To deploy your application to Elastic Beanstalk**

1. In your application's root folder, rename the `Dockerfile` to `Dockerfile.local`\. This step is required for Elastic Beanstalk to use the `Dockerfile` that contains the correct instructions for Elastic Beanstalk to build a customized Docker image on each Amazon EC2 instance in your Elastic Beanstalk environment\.
**Note**  
You do not need to perform this step if your `Dockerfile` includes instructions that modify the base Go Docker image\. You do not need to use a `Dockerfile` if your `Dockerfile` includes only a `FROM` line to specify the base image from which to build the container\. In that situation, the `Dockerfile` is redundant\.

1. Create an application source bundle\. For more information, see [Create an Application Source Bundle](applications-sourcebundle.md)\.

1. Create an Elastic Beanstalk environment to which you can deploy your application\. For step\-by\-step instructions, see [Managing and Configuring AWS Elastic Beanstalk Applications](applications.md)\. At the appropriate step, on the **Environment Type** page, in the **Predefined configuration** list, under **Preconfigured \- Docker**, choose **Go**\.