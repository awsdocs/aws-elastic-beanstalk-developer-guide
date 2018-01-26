# Load Balancer for Your AWS Elastic Beanstalk Environment<a name="using-features.managing.elb"></a>

If you've enabled load balancing, your environment is equipped with an Elastic Load Balancing load balancer to distribute traffic among the instances in your environment\.

**Note**  
The Elastic Beanstalk environment management console only supports creating and managing an Elastic Beanstalk environment with a Classic Load Balancer\. For other options, see  and \.

By default, your load balancer is configured to [listen](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/elb-listener-config.html) for HTTP traffic on port 80 and forward it to instances on the same port\. To support secure connections, you can configure your load balancer with a listener on port 443 and a TLS certificate\.

Elastic Load Balancing uses a [health check](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/elb-healthchecks.html) to determine whether the Amazon EC2 instances running your application are healthy\. The health check determines an instance's health status by making a request to a specified URL at a set interval\. If the URL returns an error message, or fails to return within a specified timeout period, the health check fails\.

If your application performs better by serving multiple requests from the same client on a single server, you can configure your load balancer to use [sticky sessions](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/elb-sticky-sessions.html)\. With sticky sessions, the load balancer adds a cookie to HTTP responses that identifies the EC2 instance that served the request\. When a subsequent request is received from the same client, the load balancer uses the cookie to send the request to the same instance\.

When an instance is removed from the load balancer because it has become unhealthy or the environment is scaling down, [connection draining](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/config-conn-drain.html) gives the instance time to complete requests prior to closing the connection between the instance and the load balancer\. You can change the amount of time given to instances to send a response, or disable connection draining completely\.

**Note**  
Connection draining is enabled by default when you create an environment with the console or the EB CLI\. For other clients, you must enable it with configuration options\. 

Advanced load balancer settings are available through configuration options that you can set with configuration files in your source code, or directly on an environment by using the Elastic Beanstalk API\. You can use these options to configure listeners on arbitrary ports, modify additional sticky session settings, and configure the load balancer to connect to EC2 instances securely\. You can also configure a load balancer to upload access logs to Amazon S3\.

## Configuring a Classic Load Balancer<a name="environments-cfg-loadbalancer-console"></a>

You can use the Elastic Beanstalk console to configure a Classic Load Balancer's ports, HTTPS certificate, and other settings\.

**To configure a load balancer in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Load balancer** configuration card, choose **Modify**\.


+ [Ports and Cross\-Zone Load Balancing](#using-features.managing.elb.ports)
+ [Connection Draining](#using-features.managing.elb.draining)
+ [Sessions](#using-features.managing.elb.sessions)
+ [Health Check](#using-features.managing.elb.healthchecks)

### Ports and Cross\-Zone Load Balancing<a name="using-features.managing.elb.ports"></a>

The basic settings for your load balancer let you configure the standard listener on port 80, a secure listener on port 443 or port 8443, and cross\-zone load balancing\.

![\[Elastic load balancer basic settings - ports and cross-zone load balancing\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-elb-loadbalancer.png)

In the **ELB listener** section, you can change the **Protocol** from **HTTP** to **TCP** if you want the load balancer to forward a request as is\. This prevents the load balancer from rewriting headers \(including `X-Forwarded-For`\)\. The technique doesn't work with sticky sessions\.

For HTTPS, you can add a secure listener with the **port** and **protocol** options of the **Secure ELB listener** section\. You must also select a certificate to use to decrypt the connections\. You can disable the standard listener if you only want to accept secure connections\.

**To turn on the secure listener port**

1. Create and upload a certificate and key to AWS Identity and Access Management \(IAM\)\.

   For more information about creating and uploading certificates, see [Managing Server Certificates](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingServerCerts.html) in *Using AWS Identity and Access Management*\.

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Load balancer** configuration card, choose **Modify**\.

1. In the **Secure ELB listener** section, select a port from the **Port** list to specify the secure listener port\.

1. For **SSL Certificate**, choose the ARN of your SSL certificate\. For example, `arn:aws:iam::123456789012:server-certificate/abc/certs/build`\)\.

1. \(Optional\) Set **Port** in the **ELB listener** section to **Off** to disable the standard listener\.

1. Choose **Save**, and then choose **Apply**\.

For more detail on configuring HTTPS and working with certificates, see \.

### Connection Draining<a name="using-features.managing.elb.draining"></a>

Use these settings to turn connection draining on or off and set the **Draining timeout** to anything up to **3600** seconds\.

![\[Elastic load balancer settings for connection draining\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-elb-draining.png)

### Sessions<a name="using-features.managing.elb.sessions"></a>

Use these settings to enable session sticking and configure the length of a session, up to a maximum of **1000000** seconds\.

![\[Elastic load balancer settings for session sticking\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-elb-sessions.png)

### Health Check<a name="using-features.managing.elb.healthchecks"></a>

Specify an **Application health check URL** to configure the load balancer to make an HTTP GET request to a specific route\. For example, type **/** to send requests to the application root, or **/health** to send requests to a resource at `/health`\. If you don't configure a health check URL, the load balancer attempts to establish a TCP connection with the instance\.

**Note**  
Configuring a health check URL doesn't affect the health check behavior of an environment's Auto Scaling group\. Instances that fail an Elastic Load Balancing health check will not automatically be replaced by Amazon EC2 Auto Scaling unless you manually configure Amazon EC2 Auto Scaling to do so\. See  for details\. 

![\[Elastic load balancer settings for health check\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-elb-healthcheck.png)

The remaining options let you customize the number of seconds between each health check \(**Health check interval**\), the number of seconds to wait for the health check to return \(**Health check timeout**\), and the number of health checks that must pass \(**Healthy check count threshold**\) or fail \(**Unhealthy check count threshold**\) before Elastic Load Balancing marks an instance as healthy or unhealthy\.

For more information about health checks and how they influence your environment's overall health, see \.

## Load Balancer Configuration Namespaces<a name="environments-cfg-loadbalancer-namespace"></a>

Elastic Beanstalk provides additional configuration options in the following namespaces that enable you to further customize the load balancer in your environment:

+ `aws:elb:healthcheck` – Configure the thresholds, check interval, and timeout for ELB health checks\.

+ `aws:elasticbeanstalk:application` – Configure the health check URL\.

+ `aws:elb:loadbalancer` – Enable cross\-zone load balancing\. Assign security groups to the load balancer and override the default security group that Elastic Beanstalk creates\. This namespace also includes deprecated options for configuring the standard and secure listeners that have been replaced by options in the `aws:elb:listener` namespace\.

+ `aws:elb:listener` – Configure the default listener on port 80, a secure listener on 443, or additional listeners for any protocol on any port\.

+ `aws:elb:policies` – Configure additional settings for your load balancer\. You can use options in this namespace to configure listeners on arbitrary ports, modify additional sticky session settings, and configure the load balancer to connect to EC2 instances securely\.

### `aws:elb:listener`<a name="environments-cfg-loadbalancer-namespace-listener"></a>

You can use `aws:elb:listener` namespaces to configure additional listeners on your load balancer\. If you specify `aws:elb:listener` as the namespace, settings apply to the default listener on port 80\. If you specify a port \(for example, `aws:elb:listener:443`\), a listener is configured on that port\.

The following example configuration file creates an HTTPS listener on port 443, assigns a certificate that the load balancer uses to terminate the secure connection, and disables the default listener on port 80\. The load balancer forwards the decrypted requests to the EC2 instances in your environment on HTTP:80\.

**Example \.ebextensions/loadbalancer\-terminatehttps\.config**  

```
option_settings:
  aws:elb:listener:443:
    ListenerProtocol: HTTPS
    SSLCertificateId: arn:aws:iam::123456789012:server-certificate/elastic-beanstalk-x509
    InstancePort: 80
    InstanceProtocol: HTTP
  aws:elb:listener:80:
    ListenerEnabled: false
```

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See  for details\.