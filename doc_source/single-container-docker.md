# Using the Docker platform<a name="single-container-docker"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

AWS Elastic Beanstalk can launch Docker environments by building an image described in a `Dockerfile` or pulling a remote Docker image\. If you're deploying a remote Docker image, you don't need to include a `Dockerfile`\. Instead, use a `Dockerrun.aws.json` file, which specifies an image to use and additional configuration options\.

**Topics**
+ [Prerequisites](#single-container-docker.prereqs)
+ [Containerize an Elastic Beanstalk application](#single-container-docker.setup)
+ [Test a container locally](#single-container-docker.test-local)
+ [Deploy a container with a Dockerfile](#single-container-docker.deploy-local)
+ [Test a remote Docker image](#single-container-docker.test-remote)
+ [Deploy a remote Docker image to Elastic Beanstalk](#single-container-docker.deploy-remote)
+ [Clean up](#single-container-docker.cleanup)
+ [Docker configuration](single-container-docker-configuration.md)
+ [Deploying a GlassFish application to the Docker platform](docker-glassfish-tutorial.md)

## Prerequisites<a name="single-container-docker.prereqs"></a>

This tutorial assumes that you have some knowledge of basic Elastic Beanstalk operations, [Using the Elastic Beanstalk command line interface \(EB CLI\)](eb-cli3.md), and Docker\. To follow this tutorial, you need a working local installation of Docker\. For more information about installing Docker, see the [Docker installation guide](https://docs.docker.com/install/)\.

If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\. This tutorial uses the EB CLI, but you can also create environments and upload applications by using the Elastic Beanstalk console\. To learn more about configuring Docker environments, see [Docker configuration](single-container-docker-configuration.md)\.

## Containerize an Elastic Beanstalk application<a name="single-container-docker.setup"></a>

For this example, we create a Docker image of the sample Flask application from [Deploying a flask application to Elastic Beanstalk](create-deploy-python-flask.md)\. The application consists of one main file, `application.py`\. We also need a `Dockerfile`\. Put both files at the root of a directory\.

```
~/eb-docker-flask/
|-- Dockerfile
|-- application.py
```

**Example `~/eb-docker-flask/application.py`**  

```
from flask import Flask

# Print a nice greeting
def say_hello(username = "World"):
    return '<p>Hello %s!</p>\n' % username

# Some bits of text for the page
header_text = '''
    <html>\n<head> <title>EB Flask Test</title> </head>\n<body>'''
instructions = '''
    <p><em>Hint</em>: This is a RESTful web service! Append a username
    to the URL (for example: <code>/Thelonious</code>) to say hello to
    someone specific.</p>\n'''
home_link = '<p><a href="/">Back</a></p>\n'
footer_text = '</body>\n</html>'

# Elastic Beanstalk looks for an 'application' that is callable by default
application = Flask(__name__)

# Add a rule for the index page
application.add_url_rule('/', 'index', (lambda: header_text +
    say_hello() + instructions + footer_text))

# Add a rule when the page is accessed with a name appended to the site
# URL
application.add_url_rule('/<username>', 'hello', (lambda username:
    header_text + say_hello(username) + home_link + footer_text))

# Run the application
if __name__ == "__main__":
    # Setting debug to True enables debug output. This line should be
    # removed before deploying a production application.
    application.debug = True
    application.run(host="0.0.0.0")
```

**Example `~/eb-docker-flask/Dockerfile`**  

```
FROM python:3.6
COPY . /app
WORKDIR /app
RUN pip install Flask==1.0.2
EXPOSE 5000
CMD ["python", "application.py"]
```

## Test a container locally<a name="single-container-docker.test-local"></a>

Use the Elastic Beanstalk CLI \(EB CLI\) to configure your local repository for deployment to Elastic Beanstalk\. Set your application's `Dockerfile` at the root of the directory\.

```
~/eb-docker-flask$ eb init -p docker application-name
```

\(Optional\) Use the eb local run command to build and run your container locally\.

```
~/eb-docker-flask$ eb local run --port 5000
```

**Note**  
To learn more about the eb local command, see [eb local](eb3-local.md)\. The command isn't supported on Windows\. Alternatively, you can build and run your container with the docker build and docker run commands\. For more information, see the [Docker documentation](https://docs.docker.com/)\.

\(Optional\) While your container is running, use the eb local open command to view your application in a web browser\. Alternatively, open [http://localhost:5000/](http://localhost:5000/) in a web browser\.

```
~/eb-docker-flask$ eb local open
```

## Deploy a container with a Dockerfile<a name="single-container-docker.deploy-local"></a>

After testing your application locally, deploy it to an Elastic Beanstalk environment\. Elastic Beanstalk uses the instructions in your `Dockerfile` to build and run the image\.

Use the eb create command to create an environment and deploy your application\.

```
~/eb-docker-flask$ eb create environment-name
```

After your environment launches, use the eb open command to view it in a web browser\.

```
~/eb-docker-flask$ eb open
```

## Test a remote Docker image<a name="single-container-docker.test-remote"></a>

Next, we build a Docker image of the Flask application from the previous section and push it to Docker Hub\.

**Note**  
The following steps create a publicly available Docker image\.

Once we've built and pushed our image, we can deploy it to Elastic Beanstalk with a `Dockerrun.aws.json` file\. To build a Docker image of the Flask application and push it to Docker Hub, run the following commands\. We're using the same directory from the previous example, but you can use any directory with your application's code\.

```
~/eb-docker-flask$ docker build -t docker-username/beanstalk-flask:latest .
~/eb-docker-flask$ docker push docker-username/beanstalk-flask:latest
```

**Note**  
Before pushing your image, you might need to run docker login\.

Now you can deploy your application using only a `Dockerrun.aws.json` file\. To learn more about `Dockerrun.aws.json` files, see [Docker configuration](single-container-docker-configuration.md)\.

Make a new directory and create a `Dockerrun.aws.json` file\.

**Example `~/remote-docker/Dockerrun.aws.json`**  

```
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "username/beanstalk-flask",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "5000"
    }
  ]
}
```

Use the EB CLI to configure your local repository for deployment to Elastic Beanstalk\.

```
~/remote-docker$ eb init -p docker application-name
```

\(Optional\) Use eb local run to build and run your container locally\. To learn more about the eb local command, see [eb local](eb3-local.md)\.

```
~/remote-docker$ eb local run --port 5000
```

\(Optional\) While your container is running, use the eb local open command to view your application in a web browser\. Alternatively, open [http://localhost:5000/](http://localhost:5000/) in a web browser\.

```
~/remote-docker$ eb local open
```

## Deploy a remote Docker image to Elastic Beanstalk<a name="single-container-docker.deploy-remote"></a>

After testing your container locally, deploy it to an Elastic Beanstalk environment\. Elastic Beanstalk uses the `Dockerrun.aws.json` file to pull and run your image\.

Use the EB CLI to create an environment and deploy your image\.

```
~/remote-docker$ eb create environment-name
```

Once your environment is launched, use eb open to view it in a web browser\.

```
~/remote-docker$ eb open
```

## Clean up<a name="single-container-docker.cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

Or, with the EB CLI:

```
~/remote-docker$ eb terminate environment-name
```