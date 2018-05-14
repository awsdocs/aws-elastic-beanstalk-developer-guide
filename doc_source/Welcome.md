# What Is AWS Elastic Beanstalk?<a name="Welcome"></a>

Amazon Web Services \(AWS\) comprises over one hundred services, each of which exposes an area of functionality\. While the variety of services offers flexibility for how you want to manage your AWS infrastructure, it can be challenging to figure out which services to use and how to provision them\.

With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without worrying about the infrastructure that runs those applications\. AWS Elastic Beanstalk reduces management complexity without restricting choice or control\. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring\. Elastic Beanstalk uses highly reliable and scalable services that are available in the [AWS Free Tier](https://aws.amazon.com/free)\.

Elastic Beanstalk supports applications developed in Java, PHP, \.NET, Node\.js, Python, and Ruby, as well as different container types for each language\. A container defines the infrastructure and software stack to be used for a given environment\. When you deploy your application, Elastic Beanstalk provisions one or more AWS resources, such as Amazon EC2 instances\. The software stack that runs on your Amazon EC2 instances depends on the container type\. For example, Elastic Beanstalk supports two container types for Node\.js: a 32\-bit Amazon Linux image and a 64\-bit Amazon Linux image\. Each runs a software stack tailored to hosting a Node\.js application\. You can interact with Elastic Beanstalk by using the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), or `eb`, a high\-level CLI designed specifically for Elastic Beanstalk\. 

To learn more about the AWS Free Usage Tier and how to deploy a sample web application in it using AWS Elastic Beanstalk, go to [Getting Started with AWS: Deploying a Web Application](http://docs.aws.amazon.com/gettingstarted/latest/deploy/welcome.html)\.

You can also perform most deployment tasks, such as changing the size of your fleet of Amazon EC2 instances or monitoring your application, directly from the Elastic Beanstalk web interface \(console\)\. 

To use Elastic Beanstalk, you create an application, upload an application version in the form of an application source bundle \(for example, a Java \.war file\) to Elastic Beanstalk, and then provide some information about the application\. Elastic Beanstalk automatically launches an environment and creates and configures the AWS resources needed to run your code\. After your environment is launched, you can then manage your environment and deploy new application versions\. The following diagram illustrates the workflow of Elastic Beanstalk\.

![\[AWS Elastic Beanstalk Flow\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-flow-00.png)

After you create and deploy your application, information about the application—including metrics, events, and environment status—is available through the AWS Management Console, APIs, or Command Line Interfaces, including the unified AWS CLI\. For step\-by\-step instructions on how to create, deploy, and manage your application using the AWS Management Console, go to [Getting Started Using Elastic Beanstalk](GettingStarted.md)\. To learn more about an Elastic Beanstalk application and its components, see [AWS Elastic Beanstalk Concepts](concepts.md)\.

Elastic Beanstalk provides developers and systems administrators an easy, fast way to deploy and manage their applications without having to worry about AWS infrastructure\. If you already know the AWS resources you want to use and how they work, you might prefer defining a template for creating AWS resources with AWS CloudFormation\. You can then use this template to repeatedly launch new AWS resources in the exact same way without having to re\-customize them\. Once your AWS resources are deployed, you can modify and update them in a controlled and predictable way, providing the same sort of version control over your AWS infrastructure that you exercise over your software\. For more information about AWS CloudFormation, see [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)\.

## Storage<a name="Welcome.storage"></a>

Elastic Beanstalk does not restrict your choice of persistent storage and database service options\. For more information on AWS storage options, go to [Storage Options in the AWS Cloud](https://aws.amazon.com/whitepapers/)\.

## Pricing<a name="Welcome.pricing"></a>

There is no additional charge for Elastic Beanstalk\. You pay only for the underlying AWS resources that your application consumes\. For details about pricing, see the [Elastic Beanstalk service detail page](https://aws.amazon.com/elasticbeanstalk)\.

## Community<a name="Welcome.community"></a>

Customers have built a wide variety of products, services, and applications on top of AWS\. Whether you are searching for ideas about what to build, looking for examples, or just want to explore, you can find many solutions at the [AWS Customer App Catalog](https://aws.amazon.com/customerapps)\. You can browse by audience, services, and technology\. We also invite you to share applications you build with the community\. Developer resources produced by the AWS community are at [https://aws\.amazon\.com/resources/](https://aws.amazon.com/resources/)\. 

## Where to Go Next<a name="Welcome.WhereToGo"></a>

This guide contains conceptual information about the Elastic Beanstalk web service, as well as information about how to use the service to deploy web applications\. Separate sections describe how to use the AWS Management console, command line interface \(CLI\) tools, and API to deploy and manage your Elastic Beanstalk environments\. This guide also documents how Elastic Beanstalk is integrated with other services provided by Amazon Web Services\.

We recommend that you first read [Getting Started Using Elastic Beanstalk](GettingStarted.md) to learn how to start using Elastic Beanstalk\. Getting Started steps you through creating, viewing, and updating your Elastic Beanstalk application, as well as editing and terminating your Elastic Beanstalk environment\. Getting Started also describes different ways you can access Elastic Beanstalk\. We also recommend that you familiarize yourself with Elastic Beanstalk concepts and terminology by reading [AWS Elastic Beanstalk Concepts](concepts.md)\.