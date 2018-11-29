# Auto Scaling Health Check Setting<a name="environmentconfig-autoscaling-healthchecktype"></a>

Amazon EC2 Auto Scaling monitors the health of each Amazon EC2 instance that it launches\. If any instance terminates unexpectedly, Amazon EC2 Auto Scaling detects the termination and launches a replacement instance\. By default, the Auto Scaling group created for your environment uses [Amazon EC2 status checks](https://docs.aws.amazon.com/autoscaling/latest/userguide/healthcheck.html)\. If an instance in your environment fails an EC2 status check, Amazon EC2 Auto Scaling takes it down and replaces it\.

EC2 status checks only cover an instance's health, not the health of your application, server, or any Docker containers running on the instance\. If your application crashes, but the instance that it runs on is still healthy, it may be kicked out of the load balancer, but Amazon EC2 Auto Scaling won't replace it automatically\. The default behavior is good for troubleshooting\. If Amazon EC2 Auto Scaling replaced the instance as soon as the application crashed, you might not realize that anything went wrong, even if it crashed quickly after starting up\.

If you want Amazon EC2 Auto Scaling to replace instances whose application has stopped responding, you can configure the Auto Scaling group to use Elastic Load Balancing health checks with a [configuration file](ebextensions.md)\. This configuration file tells the group to use the load balancer's health checks, instead of the EC2 status check, to determine an instance's health\.

**Example \.ebextensions/autoscaling\.config**  

```
Resources:
  AWSEBAutoScalingGroup:
    Type: "AWS::AutoScaling::AutoScalingGroup"
    Properties:
      HealthCheckType: ELB
      HealthCheckGracePeriod: 300
```

You can read the [CloudFormation documentation on AWS::AutoScaling::AutoScalingGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-group.html) for more information on the `HealthCheckType` and `HealthCheckGracePeriod` properties.

By default, the Elastic Load Balancing health check is configured to attempt a TCP connection to your instance over port 80\. This confirms that the web server running on the instance is accepting connections\. However, you might want to [customize the load balancer health check](using-features.managing.elb.md) to ensure that your application, and not just the web server, is in a good state\. The grace period setting sets the number of seconds that an instance can fail the health check without being terminated and replaced\. Instances can recover after being kicked out of the load balancer, so give the instance an amount of time that is appropriate for your application\.
