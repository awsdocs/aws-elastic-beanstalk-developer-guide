# Configuring your Elastic Beanstalk environment's load balancer to terminate HTTPS<a name="configuring-https-elb"></a>

To update your AWS Elastic Beanstalk environment to use HTTPS, you need to configure an HTTPS listener for the load balancer in your environment\. Two types of load balancer support an HTTPS listener: Classic Load Balancer and Application Load Balancer\.

You can use the Elastic Beanstalk console or a configuration file to configure a secure listener and assign the certificate\.

**Note**  
Single\-instance environments don't have a load balancer and don't support HTTPS termination at the load balancer\.

## Configuring a secure listener using the Elastic Beanstalk console<a name="configuring-https-elb.console"></a>

**To assign a certificate to your environment's load balancer**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Load balancer** configuration category, choose **Edit**\.
**Note**  
If the **Load balancer** configuration category doesn't have an **Edit** button, your environment doesn't have a [load balancer](using-features-managing-env-types.md#using-features.managing.changetype)\.

1. On the **Modify load balancer** page, the procedure varies depending on the type of load balancer associated with your environment\.
   + **Classic Load Balancer**

     1. Choose **Add listener**\.

     1. In the **Classic Load Balancer listener** dialog box, configure the following settings:
        + For **Listener port**, type the incoming traffic port, typically `443`\.
        + For **Listener protocol**, choose **HTTPS**\.
        + For **Instance port**, type `80`\.
        + For **Instance protocol**, choose **HTTP**\.
        + For **SSL certificate**, choose your certificate\.

     1. Choose **Add**\.
   + **Application Load Balancer**

     1. Choose **Add listener**\.

     1. In the **Application Load Balancer listener** dialog box, configure the following settings:
        + For **Port**, type the incoming traffic port, typically `443`\.
        + For **Protocol**, choose **HTTPS**\.
        + For **SSL certificate**, choose your certificate\.

     1. Choose **Add**\.
**Note**  
For Classic Load Balancer and Application Load Balancer, if the drop\-down menu doesn't show any certificates, you should create or upload a certificate for your [custom domain name](customdomains.md) in [AWS Certificate Manager \(ACM\)](https://docs.aws.amazon.com/acm/latest/userguide/) \(preferred\)\. Alternatively, upload a certificate to IAM with the AWS CLI\.
   + **Network Load Balancer**

     1. Choose **Add listener**\.

     1. In the **Network Load Balancer listener** dialog box, for **Port**, type the incoming traffic port, typically `443`\.

     1. Choose **Add**\.

1. Choose **Apply**\.

## Configuring a secure listener using a configuration file<a name="configuring-https-elb.configurationfile"></a>

You can configure a secure listener on your load balancer with one of the following [configuration files](ebextensions.md)\.

**Example \.ebextensions/securelistener\-clb\.config**  
Use this example when your environment has a Classic Load Balancer\. The example uses options in the `aws:elb:listener` namespace to configure an HTTPS listener on port 443 with the specified certificate, and to forward the decrypted traffic to the instances in your environment on port 80\.  

```
option_settings:
  aws:elb:listener:443:
    SSLCertificateId: arn:aws:acm:us-east-2:1234567890123:certificate/####################################
    ListenerProtocol: HTTPS
    InstancePort: 80
```

Replace the highlighted text with the ARN of your certificate\. The certificate can be one that you created or uploaded in AWS Certificate Manager \(ACM\) \(preferred\), or one that you uploaded to IAM with the AWS CLI\.

For more information about Classic Load Balancer configuration options, see [Classic Load Balancer configuration namespaces](environments-cfg-clb.md#environments-cfg-clb-namespace)\.

**Example \.ebextensions/securelistener\-alb\.config**  
Use this example when your environment has an Application Load Balancer\. The example uses options in the `aws:elbv2:listener` namespace to configure an HTTPS listener on port 443 with the specified certificate\. The listener routes traffic to the default process\.  

```
option_settings:
  aws:elbv2:listener:443:
    ListenerEnabled: 'true'
    Protocol: HTTPS
    SSLCertificateArns: arn:aws:acm:us-east-2:1234567890123:certificate/####################################
```

**Example \.ebextensions/securelistener\-nlb\.config**  
Use this example when your environment has a Network Load Balancer\. The example uses options in the `aws:elbv2:listener` namespace to configure a listener on port 443\. The listener routes traffic to the default process\.  

```
option_settings:
  aws:elbv2:listener:443:
    ListenerEnabled: 'true'
```

## Configuring a security group<a name="configuring-https-elb.security-group"></a>

If you configure your load balancer to forward traffic to an instance port other than port 80, you must add a rule to your security group that allows inbound traffic over the instance port from your load balancer\. If you create your environment in a custom VPC, Elastic Beanstalk adds this rule for you\.

You add this rule by adding a `Resources` key to a [configuration file](ebextensions.md) in the `.ebextensions` directory for your application\.

The following example configuration file adds an ingress rule to the `AWSEBSecurityGroup` security group\. This allows traffic on port 1000 from the load balancer's security group\.

**Example \.ebextensions/sg\-ingressfromlb\.config**  

```
Resources:
  sslSecurityGroupIngress:
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
      IpProtocol: tcp
      ToPort: 1000
      FromPort: 1000
      SourceSecurityGroupName: {"Fn::GetAtt" : ["AWSEBLoadBalancer" , "SourceSecurityGroup.GroupName"]}
```