# Configuring end\-to\-end encryption in a load\-balanced Elastic Beanstalk environment<a name="configuring-https-endtoend"></a>

Terminating secure connections at the load balancer and using HTTP on the backend might be sufficient for your application\. Network traffic between AWS resources can't be listened to by instances that are not part of the connection, even if they are running under the same account\.

However, if you are developing an application that needs to comply with strict external regulations, you might be required to secure all network connections\. You can use the Elastic Beanstalk console or [configuration files](ebextensions.md) to make your Elastic Beanstalk environment's load balancer connect to backend instances securely to meet these requirements\. The following procedure focuses on configuration files\.

First, [add a secure listener to your load balancer](configuring-https-elb.md), if you haven't already\.

You must also configure the instances in your environment to listen on the secure port and terminate HTTPS connections\. The configuration varies per platform\. See [Configuring your application to terminate HTTPS connections at the instance](https-singleinstance.md) for instructions\. You can use a [self\-signed certificate](configuring-https-ssl.md) for the EC2 instances without issue\.

Next, configure the listener to forward traffic using HTTPS on the secure port used by your application\. Use one of the following configuration files, based on the type of load balancer that your environment uses\.

**`.ebextensions/https-reencrypt-clb.config`**

Use this configuration file with a Classic Load Balancer\. In addition to configuring the load balancer, the configuration file also changes the default health check to use port 443 and HTTPS, to ensure that the load balancer can connect securely\.

```
option_settings:
  aws:elb:listener:443:
    InstancePort: 443
    InstanceProtocol: HTTPS
  aws:elasticbeanstalk:application:
    Application Healthcheck URL: HTTPS:443/
```

**`.ebextensions/https-reencrypt-alb.config`**

Use this configuration file with an Application Load Balancer\.

```
option_settings:
  aws:elbv2:listener:443:
    DefaultProcess: https
    ListenerEnabled: 'true'
    Protocol: HTTPS
  aws:elasticbeanstalk:environment:process:https:
    Port: '443'
    Protocol: HTTPS
```

**`.ebextensions/https-reencrypt-nlb.config`**

Use this configuration file with a Network Load Balancer\.

```
option_settings:
  aws:elbv2:listener:443:
    DefaultProcess: https
    ListenerEnabled: 'true'
  aws:elasticbeanstalk:environment:process:https:
    Port: '443'
```

The `DefaultProcess` option is named this way because of Application Load Balancers, which can have nondefault listeners on the same port for traffic to specific paths \(see [Application Load Balancer](environments-cfg-alb.md) for details\)\. For a Network Load Balancer the option specifies the only target process for this listener\.

In this example, we named the process `https` because it listens to secure \(HTTPS\) traffic\. The listener sends traffic to the process on the designated port using the TCP protocol, because a Network Load Balancer works only with TCP\. This is okay, because network traffic for HTTP and HTTPS is implemented on top of TCP\.

**Note**  
The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended values](command-options.md#configuration-options-recommendedvalues) for details\.

In the next task, you need to modify the load balancer's security group to allow traffic\. Depending on the [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) in which you launch your environment—the default VPC or a custom VPC—the load balancer's security group will vary\. In a default VPC, Elastic Load Balancing provides a default security group that all load balancers can use\. In an Amazon VPC that you create, Elastic Beanstalk creates a security group for the load balancer to use\.

To support both scenarios, you can create a security group and tell Elastic Beanstalk to use it\. The following configuration file creates a security group and attaches it to the load balancer\.

**`.ebextensions/https-lbsecuritygroup.config`**

```
option_settings:
  # Use the custom security group for the load balancer
  aws:elb:loadbalancer:
    SecurityGroups: '`{ "Ref" : "loadbalancersg" }`'
    ManagedSecurityGroup: '`{ "Ref" : "loadbalancersg" }`'

Resources:
  loadbalancersg:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: load balancer security group
      VpcId: vpc-########
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 443
          ToPort: 443
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
```

Replace the highlighted text with your default or custom VPC ID\. The previous example includes ingress and egress over port 80 to allow HTTP connections\. You can remove those properties if you want to allow only secure connections\.

Finally, add ingress and egress rules that allow communication over port 443 between the load balancer's security group and the instances' security group\.

**`.ebextensions/https-backendsecurity.config`**

```
Resources:
  # Add 443-inbound to instance security group (AWSEBSecurityGroup)
  httpsFromLoadBalancerSG: 
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
      IpProtocol: tcp
      ToPort: 443
      FromPort: 443
      SourceSecurityGroupId: {"Fn::GetAtt" : ["loadbalancersg", "GroupId"]}
  # Add 443-outbound to load balancer security group (loadbalancersg)
  httpsToBackendInstances: 
    Type: AWS::EC2::SecurityGroupEgress
    Properties:
      GroupId: {"Fn::GetAtt" : ["loadbalancersg", "GroupId"]}
      IpProtocol: tcp
      ToPort: 443
      FromPort: 443
      DestinationSecurityGroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
```

Doing this separately from security group creation enables you to restrict the source and destination security groups without creating a circular dependency\.

After you have completed all the previous tasks, the load balancer connects to your backend instances securely using HTTPS\. The load balancer doesn't care if your instance's certificate is self\-signed or issued by a trusted certificate authority, and will accept any certificate presented to it\.

You can change this behavior by adding policies to the load balancer that tell it to trust only a specific certificate\. The following configuration file creates two policies\. One policy specifies a public certificate, and the other tells the load balancer to only trust that certificate for connections to instance port 443\.

**`.ebextensions/https-backendauth.config`**

```
option_settings:
  # Backend Encryption Policy
  aws:elb:policies:backendencryption:
    PublicKeyPolicyNames: backendkey
    InstancePorts:  443
  # Public Key Policy
  aws:elb:policies:backendkey:
    PublicKey: |
      -----BEGIN CERTIFICATE-----
      ################################################################
      ################################################################
      ################################################################
      ################################################################
      ################################################
      -----END CERTIFICATE-----
```

Replace the highlighted text with the contents of your EC2 instance's public certificate\.