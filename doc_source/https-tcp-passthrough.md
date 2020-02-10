# Configuring your environment's load balancer for TCP Passthrough<a name="https-tcp-passthrough"></a>

If you don't want the load balancer in your AWS Elastic Beanstalk environment to decrypt HTTPS traffic, you can configure the secure listener to relay requests to backend instances as\-is\.

First [configure your environment's EC2 instances to terminate HTTPS](https-singleinstance.md)\. Test the configuration on a single instance environment to make sure everything works before adding a load balancer to the mix\.

Add a [configuration file](ebextensions.md) to your project to configure a listener on port 443 that passes TCP packets as\-is to port 443 on backend instances:

**`.ebextensions/https-lb-passthrough.config`**

```
option_settings:
  aws:elb:listener:443:
    ListenerProtocol: TCP
    InstancePort: 443
    InstanceProtocol: TCP
```

In a default [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\), you also need to add a rule to the instances' security group to allow inbound traffic on 443 from the load balancer:

**`.ebextensions/https-instance-securitygroup.config`**

```
Resources:
  443inboundfromloadbalancer:
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
      IpProtocol: tcp
      ToPort: 443
      FromPort: 443
      SourceSecurityGroupName: { "Fn::GetAtt": ["AWSEBLoadBalancer", "SourceSecurityGroup.GroupName"] }
```

In a custom VPC, Elastic Beanstalk updates the security group configuration for you\.