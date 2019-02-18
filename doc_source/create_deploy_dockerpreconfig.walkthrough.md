# Getting Started with Preconfigured Docker Containers<a name="create_deploy_dockerpreconfig.walkthrough"></a>

This section walks you through how to develop a sample application locally and then deploy your application to Elastic Beanstalk with a preconfigured Docker container\.

## Set Up Your Local Development Environment<a name="create_deploy_dockerpreconfig.walkthrough.setup"></a>

For this walkthrough we will use a Python Flask “Hello World” application\.

**To set up your develop environment**

1. Create a new folder for the sample application\.

   ```
   ~$ mkdir eb-flask-sample
   ~$ cd eb-flask-sample
   ```

1. In the application's root folder, create an `application.py` file\. In the file, include the following:  
**Example \~/eb\-flask\-sample/application\.py**  

   ```
   from flask import Flask
   app = Flask(__name__)
   
   @app.route('/')
   def hello_world():
       return 'Hello World!'
   
   if __name__ == '__main__':
       app.run()
   ```

1. In the application's root folder, create a `requirements.txt` file\. In the file, include the following:  
**Example \~/eb\-flask\-sample/requirements\.txt**  

   ```
   flask
   ```

## Develop and Test Locally<a name="create_deploy_dockerpreconfig.walkthrough.dev"></a>

**To develop a sample Python Flask application**

1. Add a `Dockerfile` to your application’s root folder\. In the file, specify the AWS Elastic Beanstalk Docker base image to be used to run your local preconfigured Docker container and against which Elastic Beanstalk runs any subsequent `Dockerfile` instructions by including the following:  
**Example \~/eb\-flask\-sample/Dockerfile**  

   ```
   # For Python 3.4
   FROM amazon/aws-eb-python:3.4.2-onbuild-3.5.1
   ```

   AWS Elastic Beanstalk also supports Docker images for Glassfish 4\.1 Java 8 and Glassfish 4\.0 Java 7\. For their Docker image names, see [AWS Elastic Beanstalk Supported Platforms](concepts.platforms.md)\. For more information about using a `Dockerfile`, see [Single Container Docker Configuration](single-container-docker-configuration.md)\.

1. Build the Docker image\.

   ```
   ~/eb-flask-sample$ docker build -t my-app-image .
   ```

1. Run the Docker container from the image\.
**Note**  
You must include the `-p` flag to map port 8080 on the container to the localhost port 3000\. Elastic Beanstalk Docker containers always expose the application on port 8080 on the container\. The `-it` flag runs the image as an interactive process\. The `-rm` flag cleans up the container file system when the container exits\. You can optionally include the `-d` flag to run the image as a daemon\.

   ```
   $ docker run -it --rm -p 3000:8080 my-app-image
   ```

1. To view the sample application, type the following URL into your web browser\.

   ```
   http://localhost:3000
   ```

## Deploy to Elastic Beanstalk<a name="create_deploy_dockerpreconfig.walkthrough.deploy"></a>

After testing your application, you are ready to deploy it to Elastic Beanstalk\.

**To deploy your application to Elastic Beanstalk**

1. In your application's root folder, rename the `Dockerfile` to `Dockerfile.local`\. This step is required for Elastic Beanstalk to use the `Dockerfile` that contains the correct instructions for Elastic Beanstalk to build a customized Docker image on each Amazon EC2 instance in your Elastic Beanstalk environment\.

1. Create an application source bundle\. For more information, see [Create an Application Source Bundle](applications-sourcebundle.md)\.

1. To create a new Elastic Beanstalk application to which you can deploy your application, see [Managing and Configuring AWS Elastic Beanstalk Applications](applications.md)\. At the appropriate step, on the **Environment Type** page, in the **Predefined configuration** list, under **Preconfigured \- Docker**, click **Python**\.