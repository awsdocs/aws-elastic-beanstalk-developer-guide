# Web server environments<a name="concepts-webserver"></a>

The following diagram shows an example Elastic Beanstalk architecture for a web server environment tier, and shows how the components in that type of environment tier work together\.

![\[AWS Elastic Beanstalk architecture diagram\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-architecture2.png)

The environment is the heart of the application\. In the diagram, the environment is shown within the top\-level solid line\. When you create an environment, Elastic Beanstalk provisions the resources required to run your application\. AWS resources created for an environment include one elastic load balancer \(ELB in the diagram\), an Auto Scaling group, and one or more Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.

Every environment has a CNAME \(URL\) that points to a load balancer\. The environment has a URL, such as `myapp.us-west-2.elasticbeanstalk.com`\. This URL is aliased in [Amazon Route 53](https://aws.amazon.com/route53/) to an Elastic Load Balancing URL—something like `abcdef-123456.us-west-2.elb.amazonaws.com`—by using a CNAME record\. [Amazon Route 53](https://aws.amazon.com/route53/) is a highly available and scalable Domain Name System \(DNS\) web service\. It provides secure and reliable routing to your infrastructure\. Your domain name that you registered with your DNS provider will forward requests to the CNAME\.

The load balancer sits in front of the Amazon EC2 instances, which are part of an Auto Scaling group\. Amazon EC2 Auto Scaling automatically starts additional Amazon EC2 instances to accommodate increasing load on your application\. If the load on your application decreases, Amazon EC2 Auto Scaling stops instances, but always leaves at least one instance running\. 

The software stack running on the Amazon EC2 instances is dependent on the *container type*\. A container type defines the infrastructure topology and software stack to be used for that environment\. For example, an Elastic Beanstalk environment with an Apache Tomcat container uses the Amazon Linux operating system, Apache web server, and Apache Tomcat software\. For a list of supported container types, see [Elastic Beanstalk supported platforms](concepts.platforms.md)\. Each Amazon EC2 instance that runs your application uses one of these container types\. In addition, a software component called the *host manager \(HM\)* runs on each Amazon EC2 instance\. The host manager is responsible for the following:
+ Deploying the application
+ Aggregating events and metrics for retrieval via the console, the API, or the command line
+ Generating instance\-level events
+ Monitoring the application log files for critical errors
+ Monitoring the application server
+ Patching instance components
+ Rotating your application's log files and publishing them to Amazon S3

The host manager reports metrics, errors and events, and server instance status, which are available via the Elastic Beanstalk console, APIs, and CLIs\.

The Amazon EC2 instances shown in the diagram are part of one *security group*\. A security group defines the firewall rules for your instances\. By default, Elastic Beanstalk defines a security group, which allows everyone to connect using port 80 \(HTTP\)\. You can define more than one security group\. For example, you can define a security group for your database server\. For more information about Amazon EC2 security groups and how to configure them for your Elastic Beanstalk application, see [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups)\.