# Load Balancer for Your AWS Elastic Beanstalk Environment<a name="using-features.managing.elb"></a>

When you [enable load balancing](using-features-managing-env-types.md#using-features.managing.changetype), AWS Elastic Beanstalk creates an [Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/) load balancer for your environment\. The load balancer distributes traffic among your environment's instances\.

Elastic Beanstalk supports these load balancer types:
+ [Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/) – The Elastic Load Balancing previous\-generation load balancer\. Routes HTTP, HTTPS, or TCP request traffic to different ports on environment instances\.
+ [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/) – An application layer load balancer\. Routes HTTP or HTTPS request traffic to different ports on environment instances based on the request path\.
+ [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/) – A network layer load balancer\. Routes TCP request traffic to different ports on environment instances\. Supports both active and passive health checks\.

By default, Elastic Beanstalk creates an Application Load Balancer for your environment when you enable load balancing with the Elastic Beanstalk console or the EB CLI\. It configures the load balancer to listen for HTTP traffic on port 80 and forward this traffic to instances on the same port\. You can choose the type of load balancer that your environment uses only during environment creation\. Later, you can change settings to manage the behavior of your running environment's load balancer, but you can't change its type\.

**Note**  
Your environment must be in a VPC with subnets in at least two Availability Zones to create an Application Load Balancer\. All new AWS accounts include default VPCs that meet this requirement\. If your environment is in a VPC with subnets in only one Availability Zone, it defaults to a Classic Load Balancer\. If you don't have any subnets, you can't enable load balancing\.

You can create and manage environments with all load balancer types using the Elastic Beanstalk console, the EB CLI [eb create](eb3-create.md) command, the Elastic Beanstalk APIs, or configuration files \([\.ebextensions](ebextensions.md)\)\.

See the following topics to learn about each Elastic Beanstalk\-supported load balancer type, its functionality, and how to configure and manage it in an Elastic Beanstalk environment, and how to configure a load balancer to [upload access logs](environments-cfg-loadbalancer-accesslogs.md) to Amazon S3\. 

**Topics**
+ [Configuring a Classic Load Balancer](environments-cfg-clb.md)
+ [Configuring an Application Load Balancer](environments-cfg-alb.md)
+ [Configuring a Network Load Balancer](environments-cfg-nlb.md)
+ [Configuring Access Logs](environments-cfg-loadbalancer-accesslogs.md)