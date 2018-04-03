# Single Container Docker Environments<a name="docker-singlecontainer-deploy"></a>

Single container Docker environments can be launched from a Dockerfile \(which describes an image to build\), a Dockerrun\.aws\.json file \(which specifies an image to use and additional Elastic Beanstalk configuration options\), or both\. These configuration files can be bundled with source code and deployed in a ZIP file\.

Get started with one of the following example applications, or see [Single Container Docker Configuration](create_deploy_docker_image.md) for details on authoring Docker configuration files for a single container environment\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **App code**, choose **Upload**\.

1. Choose **Local file**, choose **Browse**, and open the source bundle\.

1. Choose **Upload**\.

1. Choose **Review and launch**\.

1. Review the available settings and choose **Create app**\.

For detailed instructions on configuring and using the EB CLI, see [Configure the EB CLI](eb-cli3-configuration.md) and [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\.

**Topics**
+ [Sample PHP Application](#docker-singlecontainer-phpsample)
+ [Sample Python Application](#docker-singlecontainer-pythonsample)
+ [Sample Dockerfile Application](#docker-singlecontainer-dockerfilesample)
+ [Single Container Docker Configuration](create_deploy_docker_image.md)

## Sample PHP Application<a name="docker-singlecontainer-phpsample"></a>

GitHub link: [awslabs/eb\-demo\-php\-simple\-app](https://github.com/awslabs/eb-demo-php-simple-app/tree/docker-apache)

This sample is a PHP application that runs on a custom Ubuntu image defined in a Dockerfile\.

The PHP sample application uses Amazon RDS\. You may be charged for using these services\. If you are a new customer, you can make use of the AWS Free Usage Tier\. For more information about pricing, see the following:
+ [Amazon Relational Database Service \(RDS\) Pricing](https://aws.amazon.com/rds/pricing/)

## Sample Python Application<a name="docker-singlecontainer-pythonsample"></a>

GitHub link: [awslabs/eb\-py\-flask\-signup](https://github.com/awslabs/eb-py-flask-signup/tree/docker)

This sample is a Python application that runs on a custom Ubuntu image defined in a `Dockerfile`\. It also includes a `Dockerrun.aws.json` file that maps a storage volume on the container to a matching path on the host instance\.

The Python sample application uses Amazon DynamoDB, Amazon SQS, and Amazon SNS\. You may be charged for using these services\. If you are a new customer, you can make use of the AWS Free Usage Tier\. For more information about pricing, see the following:
+ [Amazon DynamoDB Pricing](https://aws.amazon.com/dynamodb/pricing/)
+ [Amazon SQS Pricing](https://aws.amazon.com/sqs/pricing/)
+ [Amazon SNS Pricing](https://aws.amazon.com/sns/pricing/)

## Sample Dockerfile Application<a name="docker-singlecontainer-dockerfilesample"></a>

This sample is a `Dockerfile` configured to download the game 2048 from GitHub and run it on nginx\.

Copy and paste the example into a file named Dockerfile and upload it instead of a source bundle when creating the environment\.

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