# Configuring a Network Load Balancer<a name="environments-cfg-nlb"></a>

When you [enable load balancing](using-features-managing-env-types.md#using-features.managing.changetype), your AWS Elastic Beanstalk environment is equipped with an Elastic Load Balancing load balancer to distribute traffic among the instances in your environment\. Elastic Load Balancing supports a few load balancer types\. To learn about them, see the [Elastic Load Balancing User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\. 

This topic describes the configuration of a [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/)\. For information about configuring all the load balancer types that Elastic Beanstalk supports, see [Load Balancer for Your AWS Elastic Beanstalk Environment](using-features.managing.elb.md)\.

**Note**  
You can choose the type of load balancer that your environment uses only during environment creation\. You can change settings to manage the behavior of your running environment's load balancer, but you can't change its type\.

## Introduction<a name="environments-cfg-nlb-intro"></a>

With a Network Load Balancer, the default listener accepts TCP requests on port 80 and distributes them to the instances in your environment\. You can configure health check behavior, push access logs from the load balancer to an Amazon Simple Storage Service \(Amazon S3\) bucket, configure the listener port, or add a listener on another port\.

**Note**  
Unlike a Classic Load Balancer or an Application Load Balancer, a Network Load Balancer can't have application layer \(layer 7\) HTTP or HTTPS listeners\. It only supports TCP listeners\. Web traffic in both HTTP and HTTPS protocols at layer 7 uses the TCP protocol at layer 4, so a Network Load Balancer listens to all web traffic on configured TCP ports\. To handle secure HTTPS traffic that travels on a different port \(typically 443\), you can configure a separate listener for this port and direct the traffic to a different target process\.

A Network Load Balancer supports active health checks\. These checks are based on messages to configured health check paths, similarly to the other load balancer types\. In addition, a Network Load Balancer supports passive health checks\. It automatically detects faulty backend instances and routes traffic only to healthy instances\.

You can set the load balancer type during environment creation using the EB CLI, the Elastic Beanstalk APIs, or `.ebextensions`, such as the one in the example [\.ebextensions/network\-load\-balancer\.config](#network-load-balancer.config)\. The console doesn't support this functionality\.

## Configuring a Network Load Balancer Using the EB CLI<a name="environments-cfg-nlb-ebcli"></a>

The EB CLI prompts you to choose a load balancer type when you run [`eb create`](eb3-create.md)\.

```
$ eb create
Enter Environment Name
(default is my-app): test-env
Enter DNS CNAME prefix
(default is my-app): test-env-DLW24ED23SF

Select a load balancer type
1) classic
2) application
3) network
(default is 1): 3
```

You can also specify a load balancer type with the `--elb-type` option\.

```
$ eb create test-env --elb-type network
```

## Network Load Balancer Namespaces<a name="environments-cfg-nlb-namespaces"></a>

You can find settings related to Network Load Balancers in the following namespaces:
+ `[aws:elasticbeanstalk:environment](command-options-general.md#command-options-general-elasticbeanstalkenvironment)` – Choose the load balancer type for the environment\. The value for a Network Load Balancer is `network`\.
+ `[aws:elbv2:loadbalancer](command-options-general.md#command-options-general-elbv2)` – Configure access logs and other settings that apply to the Network Load Balancer as a whole\.
**Note**  
The `ManagedSecurityGroup` and `SecurityGroups` settings in this namespace don't apply to a Network Load Balancer\.
+ `[aws:elbv2:listener](command-options-general.md#command-options-general-elbv2-listener)` – Configure listeners on the Network Load Balancer\. These settings map to the settings in `aws:elb:listener` for Classic Load Balancers\.
+ `[aws:elasticbeanstalk:environment:process](command-options-general.md#command-options-general-environmentprocess)` – Configure health checks and specify the port and protocol for the processes that run on your environment's instances\. The port and protocol settings map to the instance port and instance protocol settings in `aws:elb:listener` for a listener on a Classic Load Balancer\. Health check settings map to the settings in the `aws:elb:healthcheck` and `aws:elasticbeanstalk:application` namespaces\.

**Example \.ebextensions/network\-load\-balancer\.config**  
To get started with a Network Load Balancer, use a [configuration file](ebextensions.md) to set the load balancer type to `network`\.  

```
option_settings:
  aws:elasticbeanstalk:environment:
    LoadBalancerType: network
```

**Note**  
You can set the load balancer type only during environment creation\.

**Example \.ebextensions/nlb\-default\-process\.config**  
The following configuration file modifies health check settings on the default process\.  

```
option_settings:
  aws:elasticbeanstalk:environment:process:default:
    DeregistrationDelay: '20'
    HealthCheckInterval: '10'
    HealthyThresholdCount: '5'
    UnhealthyThresholdCount: '5'
    Port: '80'
    Protocol: TCP
```

**Example \.ebextensions/nlb\-secure\-listener\.config**  
The following configuration file adds a listener for secure traffic on port 443 and a matching target process that listens to port 443\.  

```
option_settings:
  aws:elbv2:listener:443:
    DefaultProcess: https
    ListenerEnabled: 'true'
  aws:elasticbeanstalk:environment:process:https:
    Port: '443'
```
The `DefaultProcess` option is named this way because of Application Load Balancers, which can have non\-default listeners on the same port for traffic to specific paths \(see [Application Load Balancer](environments-cfg-alb.md) for details\)\. For a Network Load Balancer the option specifies the only target process for this listener\.  
In this example, we named the process `https` because it listens to secure \(HTTPS\) traffic\. The listener sends traffic to the process on the designated port using the TCP protocol, because a Network Load Balancer works only with TCP\. This is OK, because network traffic for HTTP and HTTPS is implemented on top of TCP\.