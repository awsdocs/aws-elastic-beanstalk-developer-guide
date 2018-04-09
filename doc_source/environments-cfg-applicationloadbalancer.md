# Configuring an Application Load Balancer<a name="environments-cfg-applicationloadbalancer"></a>

If you've [enabled load balancing](using-features-managing-env-types.md#using-features.managing.changetype), your environment is equipped with an Elastic Load Balancing load balancer to distribute traffic among the instances in your environment\. Elastic Beanstalk supports a few Elastic Load Balancing types\. See the [Elastic Load Balancing User Guide](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/) to learn about them\. This topic describes the configuration of an Application Load Balancer\. To learn how to configure other load balancer types, see [Classic Load Balancer](using-features.managing.elb.md) and [Network Load Balancer](environments-cfg-nlb.md)\.

## Introduction<a name="environments-cfg-applicationloadbalancer-intro"></a>

An Application Load Balancer inspects traffic to identify the request's path so that it can direct requests for different paths to different destinations\.

By default, an Application Load Balancer performs the same function as a Classic Load Balancer\. The default listener accepts HTTP requests on port 80 and distributes them to the instances in your environment\. You can add a secure listener on port 443 with a certificate to decrypt HTTPS traffic, configure health check behavior, and push access logs from the load balancer to an Amazon Simple Storage Service \(Amazon S3\) bucket\.

**Note**  
Unlike a Classic Load Balancer, an Application Load Balancer cannot have non\-HTTP TCP or SSL/TLS listeners, and cannot use backend authentication to authenticate HTTPS connections between the load balancer and backend instances\.

In an AWS Elastic Beanstalk environment, you can use an Application Load Balancer to direct traffic for certain paths to a different port on your web server instances\. With a Classic Load Balancer, all traffic to a listener is routed to a single port on the backend instances\. With an Application Load Balancer, you can configure multiple *rules* on the listener to route requests to certain paths to different backend ports\.

For example, you could run a login process separate from your main application\. While your main application accepts the majority of requests and listens on port 80, your login process listens on port 5000 and accepts requests to the `/login` path\. With an Application Load Balancer, you can configure a single listener with two rules to route traffic to either port 80 or port 5000, depending on the path in the request\. One rule routes traffic to `/login` to port 5000, while the default rule routes all other traffic to port 80\.

An Application Load Balancer rule maps a request to a *target group*\. In Elastic Beanstalk, a target group is represented by a *process*, which you can configure with a protocol, port, and health check settings\. The process represents the process running on the instances in your environment\. The default process is a listener on port 80 of the reverse proxy \(nginx or Apache\) that runs in front of your application\.

**Note**  
Outside of Elastic Beanstalk, a target group maps to a group of instances\. A listener can use rules and target groups to route traffic to different instances based on the path\. Within Elastic Beanstalk, all of your instances in your environment are identical, so the distinction is made between processes listening on different ports\.

A Classic Load Balancer uses a single health check path for the entire environment\. With an Application Load Balancer, each process has a separate health check path that is monitored by the load balancer and Elastic Beanstalk\-enhanced health monitoring\.

To use an Application Load Balancer, your environment must be in a default or custom VPC, and must have a service role with the standard set of permissions\. If you have an older service role, you might need to [update the permissions](iam-instanceprofile.md#iam-instanceprofile-addperms) on it to include `elasticloadbalancing:DescribeTargetHealth` and `elasticloadbalancing:DescribeLoadBalancers`\. For more information about Application Load Balancers, see the [User Guide for Application Load Balancers](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/)\.

**Note**  
The Application Load Balancer health check doesn't take into account the Elastic Beanstalk health check path\. Instead, it uses the path provided in the `.ebextensions`, like the one in the example [\.ebextensions/alb\-default\-process\.config](#alb-default-process.config)\.

## Getting Started<a name="environments-cfg-applicationloadbalancer-getstarted"></a>

**Note**  
You can set the load balancer type only during environment creation using the EB CLI, the Elastic Beanstalk APIs, or `.ebextensions`, such as the one in the example [\.ebextensions/application\-load\-balancer\.config](#application-load-balancer.config)\. Now, you can create an Application Load Balancer from the Beanstalk console\.

The EB CLI prompts you to choose a load balancer type when you run `eb create`\.

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
(default is 1): 2
```

You can also specify a load balancer type with the `--elb-type` option\.

```
$ eb create test-env --elb-type application
```

## Application Load Balancer Namespaces<a name="environments-cfg-applicationloadbalancer-namespaces"></a>

You can find settings related to Application Load Balancers in the following namespaces:
+ `[aws:elasticbeanstalk:environment](command-options-general.md#command-options-general-elasticbeanstalkenvironment)` – Choose the load balancer type for the environment\. The value for an Application Load Balancer is `application`\.
+ `[aws:elbv2:loadbalancer](command-options-general.md#command-options-general-elbv2)` – Configure access logs and other settings that apply to the Application Load Balancer as a whole\.
+ `[aws:elbv2:listener](command-options-general.md#command-options-general-elbv2-listener)` – Configure listeners on the Application Load Balancer\. These settings map to the settings in `aws:elb:listener` for Classic Load Balancers\.
+ `[aws:elbv2:listenerrule](command-options-general.md#command-options-general-elbv2-listenerrule)` – Configure rules that route traffic to different processes, depending on the request path\. Rules are unique to Application Load Balancers\.
+ `[aws:elasticbeanstalk:environment:process](command-options-general.md#command-options-general-environmentprocess)` – Configure health checks and specify the port and protocol for the processes that run on your environment's instances\. The port and protocol settings map to the instance port and instance protocol settings in `aws:elb:listener` for a listener on a Classic Load Balancer\. Health check settings map to the settings in the `aws:elb:healthcheck` and `aws:elasticbeanstalk:application` namespaces\.

**Example \.ebextensions/application\-load\-balancer\.config**  
To get started with an Application Load Balancer, use a [configuration file](ebextensions.md) to set the load balancer type to `application`\.  

```
option_settings:
  aws:elasticbeanstalk:environment:
    LoadBalancerType: application
```

**Note**  
You can set the load balancer type only during environment creation\.

**Example \.ebextensions/alb\-access\-logs\.config**  
The following configuration file enables access log uploads for an environment with an Application Load Balancer\.  

```
option_settings:
  aws:elbv2:loadbalancer:
    AccessLogsS3Bucket: my-bucket
    AccessLogsS3Enabled: 'true'
    AccessLogsS3Prefix: beanstalk-alb
```

**Example \.ebextensions/alb\-default\-process\.config**  
The following configuration file modifies health check and stickiness settings on the default process\.  

```
option_settings:
  aws:elasticbeanstalk:environment:process:default:
    DeregistrationDelay: '20'
    HealthCheckInterval: '15'
    HealthCheckPath: /
    HealthCheckTimeout: '5'
    HealthyThresholdCount: '3'
    UnhealthyThresholdCount: '5'
    Port: '80'
    Protocol: HTTP
    StickinessEnabled: 'true'
    StickinessLBCookieDuration: '43200'
```

**Example \.ebextensions/alb\-secure\-listener\.config**  
The following configuration file adds a secure listener and a matching process on port 443\.  

```
option_settings:
  aws:elbv2:listener:443:
    DefaultProcess: https
    ListenerEnabled: 'true'
    Protocol: HTTPS
    SSLCertificateArns: arn:aws:acm:us-east-2:0123456789012:certificate/21324896-0fa4-412b-bf6f-f362d6eb6dd7
  aws:elasticbeanstalk:environment:process:https:
    Port: '443'
    Protocol: HTTPS
```

**Example \.ebextensions/alb\-admin\-rule\.config**  
The following configuration file adds a secure listener with a rule that routes traffic with a request path of `/admin` to a process named `admin` that listens on port 4443\.  

```
option_settings:
  aws:elbv2:listener:443:
    DefaultProcess: https
    ListenerEnabled: 'true'
    Protocol: HTTPS
    Rules: admin
    SSLCertificateArns: arn:aws:acm:us-east-2:0123456789012:certificate/21324896-0fa4-412b-bf6f-f362d6eb6dd7
  aws:elasticbeanstalk:environment:process:https:
    Port: '443'
    Protocol: HTTPS
  aws:elasticbeanstalk:environment:process:admin:
    HealthCheckPath: /admin
    Port: '4443'
    Protocol: HTTPS
  aws:elbv2:listenerrule:admin:
    PathPatterns: /admin/*
    Priority: 1
    Process: admin
```
