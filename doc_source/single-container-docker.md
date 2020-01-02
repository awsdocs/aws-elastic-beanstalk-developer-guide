# Single Container Docker Environments<a name="single-container-docker"></a>

AWS Elastic Beanstalk can launch single container Docker environments by building an image described in a `Dockerfile` or pulling a remote Docker image\. If you're deploying a remote Docker image, you don't need to include a `Dockerfile`\. Instead, use a `Dockerrun.aws.json` file, which specifies an image to use and additional configuration options\.

**Topics**
+ [Prerequisites](#single-container-docker.prereqs)
+ [Containerize an Elastic Beanstalk Application](#single-container-docker.setup)
+ [Test a Container Locally](#single-container-docker.test-local)
+ [Deploy a Container with a Dockerfile](#single-container-docker.deploy-local)
+ [Test a Remote Docker Image](#single-container-docker.test-remote)
+ [Deploy a Remote Docker Image to Elastic Beanstalk](#single-container-docker.deploy-remote)
+ [Clean Up](#single-container-docker.cleanup)
+ [Single Container Docker Configuration](single-container-docker-configuration.md)

## Prerequisites<a name="single-container-docker.prereqs"></a>

This tutorial assumes that you have some knowledge of basic Elastic Beanstalk operations, [Using the Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md), and Docker\. To follow this tutorial, you need a working local installation of Docker\. For more information about installing Docker, see the [Docker installation guide](https://docs.docker.com/install/)\.

If you haven't already, follow the instructions in [Getting Started Using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\. This tutorial uses the EB CLI, but you can also create environments and upload applications by using the Elastic Beanstalk console\. To learn more about configuring single container Docker environments, see [Single Container Docker Configuration](single-container-docker-configuration.md)\.

## Containerize an Elastic Beanstalk Application<a name="single-container-docker.setup"></a>

For this example, we create a Docker image of the sample Flask application from [Deploying a Flask Application to Elastic Beanstalk](create-deploy-python-flask.md)\. The application consists of one main file, `application.py`\. We also need a `Dockerfile`\. Put both files at the root of a directory\.

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

## Test a Container Locally<a name="single-container-docker.test-local"></a>

Use the Elastic Beanstalk CLI \(EB CLI\) to configure your local repository for deployment to Elastic Beanstalk\. Set your application's `Dockerfile` at the root of the directory\.

```
~/eb-docker-flask$ eb init -p docker application-name
```

\(Optional\) Use eb local run to build and run your container locally\. To learn more about the eb local command, see [eb local](eb3-local.md)\. The eb local command isn't supported on Windows\. Alternatively, you can build and run your container with the docker build and docker run commands\. For more information, see the [Docker documentation](https://docs.docker.com/)\.

```
~/eb-docker-flask$ eb local run --port 5000
```

\(Optional\) While your container is running, use the eb local open command to view your application in a web browser\. Alternatively, open [http://localhost:5000/](http://localhost:5000/) in a web browser\.

```
~/eb-docker-flask$ eb local open
```

## Deploy a Container with a Dockerfile<a name="single-container-docker.deploy-local"></a>

After testing your application locally, deploy it to an Elastic Beanstalk environment\. Elastic Beanstalk uses the instructions in your `Dockerfile` to build and run the image\.

Use the EB CLI to create an environment and deploy your application\.

```
~/eb-docker-flask$ eb create environment-name
```

Once your environment has launched, use eb open to view it in a web browser\.

```
~/eb-docker-flask$ eb open
```

## Test a Remote Docker Image<a name="single-container-docker.test-remote"></a>

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

Now you can deploy your application using only a `Dockerrun.aws.json` file\. To learn more about `Dockerrun.aws.json` files, see [Single Container Docker Configuration](single-container-docker-configuration.md)\.

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

## Deploy a Remote Docker Image to Elastic Beanstalk<a name="single-container-docker.deploy-remote"></a>

After testing your container locally, deploy it to an Elastic Beanstalk environment\. Elastic Beanstalk uses the `Dockerrun.aws.json` file to pull and run your image\.

Use the EB CLI to create an environment and deploy your image\.

```
~/remote-docker$ eb create environment-name
```

Once your environment is launched, use eb open to view it in a web browser\.

```
~/remote-docker$ eb open
```

## Clean Up<a name="single-container-docker.cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then choose **Terminate Environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

Or, with the EB CLI:

```
~/remote-docker$ eb terminate environment-name
```