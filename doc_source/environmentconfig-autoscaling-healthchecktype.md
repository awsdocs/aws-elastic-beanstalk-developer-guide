# Auto Scaling Health Check Setting<a name="environmentconfig-autoscaling-healthchecktype"></a>

Auto Scaling monitors the health of each Amazon EC2 instance that it launches\. If any instance terminates unexpectedly, Auto Scaling detects the termination and launches a replacement instance\. By default, the Auto Scaling created for your environment uses [Amazon EC2 status checks](http://docs.aws.amazon.com/autoscaling/latest/userguide/healthcheck.html)\. If an instance in your environment fails an EC2 status check, it is taken down and replaced by Auto Scaling\.

EC2 status checks only cover an instance's health, not the health of your application, server, or any Docker containers running on the instance\. If your application crashes, but the instance that it runs on is still healthy, it may be kicked out of the load balancer, but it won't be replaced automatically by Auto Scaling\. The default behavior is good for troubleshooting; if Auto Scaling replaced the instance as soon as the application crashed, you may not realize that anything went wrong, even if it crashed quickly after starting up\.

If you would like Auto Scaling to replace instances whose application has stopped responding, you can configure the Auto Scaling group to use Elastic Load Balancing health checks with a configuration file\. This configuration file tells the group to use the load balancer's health checks to determine an instance's health, instead of the EC2 status check\.

**Example \.ebextensions/autoscaling\.config**  

```
Resources:
  AWSEBAutoScalingGroup:
    Type: "AWS::AutoScaling::AutoScalingGroup"
    Properties:
      HealthCheckType: ELB
      HealthCheckGracePeriod: 300
```

By default, the Elastic Load Balancing health check is configured to attempt a TCP connection to your instance over port 80\. This confirms that the web server running on the instance is accepting connections, but you may want to customize the load balancer health check to make sure that your application is in a good state, not just the web server\. The grace period setting sets the number of seconds that an instance can fail the health check without being terminated and replaced\. Instances may recover after being kicked out of the load balancer, so give the instance an amount of time that is appropriate for your application\.