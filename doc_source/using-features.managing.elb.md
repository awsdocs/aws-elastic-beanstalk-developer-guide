# Load Balancer for Your AWS Elastic Beanstalk Environment<a name="using-features.managing.elb"></a>

When you [enable load balancing](using-features-managing-env-types.md#using-features.managing.changetype), AWS Elastic Beanstalk creates an [Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/) load balancer for your environment\. The load balancer distributes traffic among your environment's instances\.

Elastic Beanstalk supports these load balancer types:
+ [Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/) – The Elastic Load Balancing previous\-generation load balancer\. Routes HTTP, HTTPS, or TCP request traffic to different ports on environment instances\.
+ [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/) – An application layer load balancer\. Routes HTTP or HTTPS request traffic to different ports on environment instances based on the request path\.
+ [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/) – A network layer load balancer\. Routes TCP request traffic to different ports on environment instances\. Supports both active and passive health checks\.

By default, Elastic Beanstalk creates a Classic Load Balancer for your environment\. It configures the load balancer to listen for HTTP traffic on port 80 and forward this traffic to instances on the same port\. You can choose the type of load balancer that your environment uses only during environment creation\. You can change settings to manage the behavior of your running environment's load balancer, but you can't change its type\.

The Elastic Beanstalk console supports creating and managing an Elastic Beanstalk environment with either a Classic Load Balancer or an Application Load Balancer\. You can create and manage environments with all load balancer types using the EB CLI [`eb create`](eb3-create.md) command, the Elastic Beanstalk APIs, or configuration files \([\.ebextensions](ebextensions.md)\)\.

We provide a topic for each Elastic Beanstalk\-supported load balancer type that describes its functionality and how to configure and manage it in an Elastic Beanstalk environment\. You can also configure a load balancer to [upload access logs](environments-cfg-loadbalancer-accesslogs.md) to Amazon S3\. See the following:

**Topics**
+ [Configuring a Classic Load Balancer](environments-cfg-clb.md)
+ [Configuring an Application Load Balancer](environments-cfg-alb.md)
+ [Configuring a Network Load Balancer](environments-cfg-nlb.md)
+ [Configuring Access Logs](environments-cfg-loadbalancer-accesslogs.md)