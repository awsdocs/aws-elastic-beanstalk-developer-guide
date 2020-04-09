# What is AWS Elastic Beanstalk?<a name="Welcome"></a>

Amazon Web Services \(AWS\) comprises over one hundred services, each of which exposes an area of functionality\. While the variety of services offers flexibility for how you want to manage your AWS infrastructure, it can be challenging to figure out which services to use and how to provision them\.

With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications\. Elastic Beanstalk reduces management complexity without restricting choice or control\. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring\.

Elastic Beanstalk supports applications developed in Go, Java, \.NET, Node\.js, PHP, Python, and Ruby\. When you deploy your application, Elastic Beanstalk builds the selected supported platform version and provisions one or more AWS resources, such as Amazon EC2 instances, to run your application\.

You can interact with Elastic Beanstalk by using the Elastic Beanstalk console, the AWS Command Line Interface \(AWS CLI\), or eb, a high\-level CLI designed specifically for Elastic Beanstalk\.

To learn more about how to deploy a sample web application using Elastic Beanstalk, see [Getting Started with AWS: Deploying a Web App](https://docs.aws.amazon.com/gettingstarted/latest/deploy/)\.

You can also perform most deployment tasks, such as changing the size of your fleet of Amazon EC2 instances or monitoring your application, directly from the Elastic Beanstalk web interface \(console\)\. 

To use Elastic Beanstalk, you create an application, upload an application version in the form of an application source bundle \(for example, a Java \.war file\) to Elastic Beanstalk, and then provide some information about the application\. Elastic Beanstalk automatically launches an environment and creates and configures the AWS resources needed to run your code\. After your environment is launched, you can then manage your environment and deploy new application versions\. The following diagram illustrates the workflow of Elastic Beanstalk\.

![\[Elastic Beanstalk flow\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-flow-00.png)

After you create and deploy your application, information about the application—including metrics, events, and environment status—is available through the Elastic Beanstalk console, APIs, or Command Line Interfaces, including the unified AWS CLI\.

## Pricing<a name="Welcome.pricing"></a>

There is no additional charge for Elastic Beanstalk\. You pay only for the underlying AWS resources that your application consumes\. For details about pricing, see the [Elastic Beanstalk service detail page](https://aws.amazon.com/elasticbeanstalk)\.

## Where to go next<a name="Welcome.WhereToGo"></a>

This guide contains conceptual information about the Elastic Beanstalk web service, as well as information about how to use the service to deploy web applications\. Separate sections describe how to use the Elastic Beanstalk console, command line interface \(CLI\) tools, and API to deploy and manage your Elastic Beanstalk environments\. This guide also documents how Elastic Beanstalk is integrated with other services provided by Amazon Web Services\.

We recommend that you first read [Getting started using Elastic Beanstalk](GettingStarted.md) to learn how to start using Elastic Beanstalk\. *Getting Started* steps you through creating, viewing, and updating your Elastic Beanstalk application, as well as editing and terminating your Elastic Beanstalk environment\. *Getting Started* also describes different ways you can access Elastic Beanstalk\.

To learn more about an Elastic Beanstalk application and its components, see the following pages\.
+ [Elastic Beanstalk concepts](concepts.md)
+ [Elastic Beanstalk platforms glossary](platforms-glossary.md)
+ [Shared responsibility model for Elastic Beanstalk platform maintenance](platforms-shared-responsibility.md)
+ [Elastic Beanstalk platform support policy](platforms-support-policy.md)