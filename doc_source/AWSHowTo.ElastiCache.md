# Using Elastic Beanstalk with Amazon ElastiCache<a name="AWSHowTo.ElastiCache"></a>

Amazon ElastiCache is a web service that enables setting up, managing, and scaling distributed in\-memory cache environments in the cloud\. It provides a high\-performance, scalable, and cost\-effective in\-memory cache, while removing the complexity associated with deploying and managing a distributed cache environment\. ElastiCache is protocol\-compliant with Redis and Memcached, so the code, applications, and most popular tools that you use today with your existing Redis and Memcached environments will work seamlessly with the service\. For more information about ElastiCache, go to the [Amazon ElastiCache](https://aws.amazon.com/elasticache/) product page\.

**To use Elastic Beanstalk with Amazon ElastiCache**

1. Create an ElastiCache cluster\. 
   + For instructions on how to create an ElastiCache cluster with Redis, go to [Getting Started with Amazon ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/GettingStarted.html) in the *ElastiCache for Redis User Guide*\.
   + For instructions on how to create an ElastiCache cluster with Memcached, go to [Getting Started with Amazon ElastiCache for Memcached](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/GettingStarted.html) in the *ElastiCache for Memcached User Guide*\.

1. Configure your ElastiCache Security Group to allow access from the Amazon EC2 security group used by your Elastic Beanstalk application\. For instructions on how to find the name of your EC2 security group using the AWS Management Console, see [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups) on the *EC2 Instances* document page\. 
   + For more information on Redis, go to [Authorize Access](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/GettingStarted.AuthorizeAccess.html) in the *ElastiCache for Redis User Guide*\.
   + For more information on Memcached, go to [Authorize Access](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/GettingStarted.AuthorizeAccess.html) in the *ElastiCache for Memcached User Guide*\.

You can use configuration files to customize your Elastic Beanstalk environment to use ElastiCache\. For configuration file examples that integrate ElastiCache with Elastic Beanstalk, see [Example: ElastiCache](customize-environment-resources-elasticache.md)\. 