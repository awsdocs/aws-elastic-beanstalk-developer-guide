# Configuring Elastic Load Balancing using AWS Toolkit for Eclipse<a name="create_deploy_Java.managingappenv.elb"></a>

Elastic Load Balancing is an Amazon web service that improves the availability and scalability of your application\. With Elastic Load Balancing, you can distribute application loads between two or more Amazon EC2 instances\. Elastic Load Balancing improves availability through redundancy, and it supports traffic growth for your application\. 

Elastic Load Balancing automatically distributes and balances incoming application traffic among all the EC2 server instances you are running\. The service also makes it easy to add new instances when you need to increase the capacity of your application\.

Elastic Beanstalk automatically provisions Elastic Load Balancing when you deploy an application\. Under **Load Balancing**, on the **Configuration** tab for your environment inside the Toolkit for Eclipse, you can edit the Elastic Beanstalk environment's load balancing configuration\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-lb.png)

The following sections describe the Elastic Load Balancing parameters you can configure for your application\.

## Ports<a name="create_deploy_Java.managingappenv.elb.ports"></a>

The load balancer provisioned to handle requests for your Elastic Beanstalk application sends requests to the Amazon EC2 instances that are running your application\. The provisioned load balancer can listen for requests on HTTP and HTTPS ports and route requests to the Amazon EC2 instances in your AWS Elastic Beanstalk application\. By default, the load balancer handles requests on the HTTP port\. At least one of the ports \(either HTTP or HTTPS\) must be turned on\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-lb-port.png)

**Important**  
Make sure that the port you specified is not locked down; otherwise, users will not be able to connect to your Elastic Beanstalk application\.

### Controlling the HTTP port<a name="create_deploy_Java.managingappenv.elb.ports.http"></a>

To turn off the HTTP port, you select OFF for **HTTP Listener Port**\. To turn on the HTTP port, you select an HTTP port \(for example, **80**\)\.

**Note**  
To access your environment using a port other than the default port 80, such as port 8080, add a listener to the existing load balancer and configure the new listener to listen on that port\.  
For example, using the [AWS CLI for Classic load balancers](https://docs.aws.amazon.com/cli/latest/reference/elb/create-load-balancer-listeners.html), type the following command, replacing *LOAD\_BALANCER\_NAME* with the name of your load balancer for Elastic Beanstalk\.  

```
aws elb create-load-balancer-listeners --load-balancer-name LOAD_BALANCER_NAME --listeners "Protocol=HTTP, LoadBalancerPort=8080, InstanceProtocol=HTTP, InstancePort=80"
```
For example, using the [AWS CLI for Application Load Balancers](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html), type the following command, replacing *LOAD\_BALANCER\_ARN* with the ARN of your load balancer for Elastic Beanstalk\.  

```
aws elbv2 create-listener --load-balancer-arn LOAD_BALANCER_ARN --protocol HTTP --port 8080
```
If you want Elastic Beanstalk to monitor your environment, do not remove the listener on port 80\.

### Controlling the HTTPS port<a name="create_deploy_Java.managingappenv.elb.ports.https"></a>

Elastic Load Balancing supports the HTTPS/TLS protocol to enable traffic encryption for client connections to the load balancer\. Connections from the load balancer to the EC2 instances are done using plain text\. By default, the HTTPS port is turned off\.

**To turn on the HTTPS port**

1. Create a new certificate using AWS Certificate Manager \(ACM\) or upload a certificate and key to AWS Identity and Access Management \(IAM\)\. For more information about requesting an ACM certificate, see [Request a Certificate](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request.html) in the *AWS Certificate Manager User Guide*\. For more information about importing third\-party certificates into ACM, see [Importing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html) in the *AWS Certificate Manager User Guide*\. If ACM isn't [available in your AWS Region](https://docs.aws.amazon.com/general/latest/gr/acm.html), use AWS Identity and Access Management \(IAM\) to upload a third\-party certificate\. The ACM and IAM services store the certificate and provide an Amazon Resource Name \(ARN\) for the SSL certificate\. For more information about creating and uploading certificates to IAM, see [Working with Server Certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingServerCerts.html) in *IAM User Guide*\.

1. Specify the HTTPS port by selecting a port from the **HTTPS Listener Port** drop\-down list\.

1. In the **SSL Certificate ID** text box, enter the Amazon Resources Name \(ARN\) of your SSL certificate\. For example, **arn:aws:iam::123456789012:server\-certificate/abc/certs/build** or **arn:aws:acm:us\-east\-2:123456789012:certificate/12345678\-12ab\-34cd\-56ef\-12345678**\. Use the SSL certificate that you created and uploaded in step 1\.

To turn off the HTTPS port, select **OFF** for **HTTPS Listener Port**\.

## Health checks<a name="create_deploy_Java.managingappenv.elb.healthchecks"></a>

You can control the settings for the health check using the **EC2 Instance Health Check** section of the **Load Balancing** panel\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-lb-healthcheck.png)

The following list describes the health check parameters you can set for your application\. 
+ To determine instance health, Elastic Beanstalk looks for a 200 response code on a URL it queries\. By default, Elastic Beanstalk checks TCP:80 for nonlegacy containers and HTTP:80 for legacy containers\. You can override to match an existing resource in your application \(e\.g\., **/myapp/index\.jsp**\) by entering it in the **Application Health Check URL** box\. If you override the default URL, Elastic Beanstalk uses HTTP to query the resource\. To check if you are using a legacy container type, see [Why are some platform versions marked legacy?](using-features.migration.md#using-features.migration.why) 
+ For **Health Check Interval \(seconds\)**, enter the number of seconds between your application's Amazon EC2 instances health checks\. 
+ For **Health Check Timeout**, specify the number of seconds for Elastic Load Balancing to wait for a response before it considers an instance unresponsive\. 
+ Use the **Healthy Check Count Threshold** and **Unhealthy Check Count Threshold** boxes, specify the number of consecutive successful or unsuccessful URL probes before Elastic Load Balancing changes the instance health status\. For example, specifying 5 in the **Unhealthy Check Count Threshold** text box means that the URL would have to return an error message or timeout five consecutive times before Elastic Load Balancing considers the health check "failed\." 

## Sessions<a name="create_deploy_Java.managingappenv.elb.sessions"></a>

By default, a load balancer routes each request independently to the server instance with the smallest load\. By comparison, a sticky session binds a user's session to a specific server instance so that all requests coming from the user during the session are sent to the same server instance\. 

Elastic Beanstalk uses load balancer–generated HTTP cookies when sticky sessions are enabled for an application\. The load balancer uses a special load balancer–generated cookie to track the application instance for each request\. When the load balancer receives a request, it first checks to see if this cookie is present in the request\. If so, the request is sent to the application instance specified in the cookie\. If it finds no cookie, the load balancer chooses an application instance based on the existing load balancing algorithm\. A cookie is inserted into the response for binding subsequent requests from the same user to that application instance\. The policy configuration defines a cookie expiry, which establishes the duration of validity for each cookie\. 

Under **Load Balancer** in the **Sessions** section, specify whether or not the load balancer for your application allows session stickiness and the duration for each cookie\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-lb-sessions.png)

For more information on Elastic Load Balancing, see the [Elastic Load Balancing Developer Guide](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/)\. 