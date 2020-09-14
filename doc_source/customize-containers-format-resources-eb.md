# Modifying the resources that Elastic Beanstalk creates for your environment<a name="customize-containers-format-resources-eb"></a>

The resources that Elastic Beanstalk creates for your environment have names\. You can use these names to get information about the resources with a [function](ebextensions-functions.md), or modify properties on the resources to customize their behavior\.

Web server environments have the following resources\.

**Web server environments**
+ `AWSEBAutoScalingGroup` \([AWS::AutoScaling::AutoScalingGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-group.html)\) – The Auto Scaling group attached to your environment\.
+ One of the following two resources\.
  + `AWSEBAutoScalingLaunchConfiguration` \([AWS::AutoScaling::LaunchConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-as-launchconfig.html)\) – The launch configuration attached to your environment's Auto Scaling group\.
  + `AWSEBEC2LaunchTemplate` \([AWS::EC2::LaunchTemplate](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-launchtemplate.html)\) – The Amazon EC2 launch template used by your environment's Auto Scaling group\.
**Note**  
If your environment uses functionality that requires Amazon EC2 launch templates, and your user policy lacks the required permissions, creating or updating the environment might fail\. Use the **AWSElasticBeanstalkFullAccess** [managed user policy](AWSHowTo.iam.managed-policies.md), or add the required permissions to your [custom policy](AWSHowTo.iam.managed-policies.md#AWSHowTo.iam.policies)\.
+ `AWSEBEnvironmentName` \([AWS::ElasticBeanstalk::Environment](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-beanstalk-environment.html)\) – Your environment\.
+ `AWSEBSecurityGroup` \([AWS::EC2::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)\) – The security group attached to your Auto Scaling group\.
+ `AWSEBRDSDatabase` \([AWS::RDS::DBInstance](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-rds-database-instance.html)\) – The Amazon RDS DB instance attached to your environment \(if applicable\)\.

In a load\-balanced environment, you can access additional resources related to the load balancer\. Classic load balancers have a resource for the load balancer and one for the security group attached to it\. Application and network load balancers have additional resources for the load balancer's default listener, listener rule, and target group\.

**Load\-balanced environments**
+ `AWSEBLoadBalancer` \([AWS::ElasticLoadBalancing::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-elb.html)\) – Your environment's classic load balancer\.
+ `AWSEBV2LoadBalancer` \([AWS::ElasticLoadBalancingV2::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-loadbalancer.html)\) – Your environment's application or network load balancer\.
+ `AWSEBLoadBalancerSecurityGroup` \([AWS::EC2::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)\) – In a custom [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) only, the name of the security group that Elastic Beanstalk creates for the load balancer\. In a default VPC or EC2 classic, Elastic Load Balancing assigns a default security group to the load balancer\.
+ `AWSEBV2LoadBalancerListener` \([AWS::ElasticLoadBalancingV2::Listener](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-listener.html)\) – A listener that allows the load balancer to check for connection requests and forward them to one or more target groups\.
+ `AWSEBV2LoadBalancerListenerRule` \([AWS::ElasticLoadBalancingV2::ListenerRule](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-listenerrule.html)\) – Defines which requests an Elastic Load Balancing listener takes action on and the action that it takes\.
+ `AWSEBV2LoadBalancerTargetGroup` \([AWS::ElasticLoadBalancingV2::TargetGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-targetgroup.html)\) – An Elastic Load Balancing target group that routes requests to one or more registered targets, such as Amazon EC2 instances\.

Worker environments have resources for the SQS queue that buffers incoming requests, and a Amazon DynamoDB table that the instances use for leader election\.

**Worker environments**
+ `AWSEBWorkerQueue` \([AWS::SQS::Queue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sqs-queues.html)\) – The Amazon SQS queue from which the daemon pulls requests that need to be processed\.
+ `AWSEBWorkerDeadLetterQueue` \([AWS::SQS::Queue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sqs-queues.html)\) – The Amazon SQS queue that stores messages that cannot be delivered or otherwise were not successfully processed by the daemon\.
+ `AWSEBWorkerCronLeaderRegistry` \([AWS::DynamoDB::Table](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-table.html)\) – The Amazon DynamoDB table that is the internal registry used by the daemon for periodic tasks\.