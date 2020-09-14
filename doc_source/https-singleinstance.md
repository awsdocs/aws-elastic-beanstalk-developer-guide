# Configuring your application to terminate HTTPS connections at the instance<a name="https-singleinstance"></a>

You can use [configuration files](ebextensions.md) to configure the proxy server that passes traffic to your application to terminate HTTPS connections\. This is useful if you want to use HTTPS with a single instance environment, or if you configure your load balancer to pass traffic through without decrypting it\.

To enable HTTPS, you must allow incoming traffic on port 443 to the EC2 instance that your Elastic Beanstalk application is running on\. You do this by using the `Resources` key in the configuration file to add a rule for port 443 to the ingress rules for the AWSEBSecurityGroup security group\.

The following snippet adds an ingress rule to the `AWSEBSecurityGroup` security group that opens port 443 to all traffic for a single instance environment:

**`.ebextensions/https-instance-securitygroup.config`**

```
Resources:
  sslSecurityGroupIngress: 
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
      IpProtocol: tcp
      ToPort: 443
      FromPort: 443
      CidrIp: 0.0.0.0/0
```

In a load\-balanced environment in a default [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\), you can modify this policy to only accept traffic from the load balancer\. See [Configuring end\-to\-end encryption in a load\-balanced Elastic Beanstalk environment](configuring-https-endtoend.md) for an example\.

**Topics**
+ [Terminating HTTPS on EC2 instances running Docker](https-singleinstance-docker.md)
+ [Terminating HTTPS on EC2 instances running Go](https-singleinstance-go.md)
+ [Terminating HTTPS on EC2 instances running Java SE](https-singleinstance-java.md)
+ [Terminating HTTPS on EC2 instances running Node\.js](https-singleinstance-nodejs.md)
+ [Terminating HTTPS on EC2 instances running PHP](https-singleinstance-php.md)
+ [Terminating HTTPS on EC2 instances running Python](https-singleinstance-python.md)
+ [Terminating HTTPS on EC2 instances running Ruby](https-singleinstance-ruby.md)
+ [Terminating HTTPS on EC2 instances running Tomcat](https-singleinstance-tomcat.md)
+ [Terminating HTTPS on Amazon EC2 instances running \.NET Core on Linux](https-singleinstance-dotnet-linux.md)
+ [Terminating HTTPS on Amazon EC2 instances running \.NET](SSLNET.SingleInstance.md)