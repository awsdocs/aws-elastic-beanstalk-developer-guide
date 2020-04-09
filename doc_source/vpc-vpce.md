# Using Elastic Beanstalk with VPC endpoints<a name="vpc-vpce"></a>

A *VPC endpoint* enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink, without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. 

Instances in your VPC don't require public IP addresses to communicate with resources in the service\. Traffic between your VPC and the other service doesn't leave the Amazon network\. For complete information about VPC endpoints, see [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\.

AWS Elastic Beanstalk supports AWS PrivateLink, which provides private connectivity to the Elastic Beanstalk service and eliminates exposure of traffic to the public internet\. To enable your application to send requests to Elastic Beanstalk using AWS PrivateLink, you configure a type of VPC endpoint known as an *interface VPC endpoint* \(interface endpoint\)\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

**Note**  
Elastic Beanstalk supports AWS PrivateLink and interface VPC endpoints in a limited number of AWS Regions\. We're working to extend support to more AWS Regions in the near future\.

## Setting up a VPC endpoint for Elastic Beanstalk<a name="vpc-vpce.eb"></a>

To create the interface VPC endpoint for the Elastic Beanstalk service in your VPC, follow the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure\. For **Service Name**, choose **com\.amazonaws\.*region*\.elasticbeanstalk**\.

If your VPC is configured with public internet access, your application can still access Elastic Beanstalk over the internet using the `elasticbeanstalk.region.amazonaws.com` public endpoint\. You can prevent this by ensuring that **Enable DNS name** is enabled during endpoint creation \(true by default\)\. This adds a DNS entry in your VPC that maps the public service endpoint to the interface VPC endpoint\.

## Setting up a VPC endpoint for enhanced health<a name="vpc-vpce.healthd"></a>

If you enabled [enhanced health reporting](health-enhanced.md) for your environment, you can configure enhanced health information to be sent over AWS PrivateLink too\. Enhanced health information is sent by the `healthd` daemon, an Elastic Beanstalk component on your environment instances, to a separate Elastic Beanstalk enhanced health service\. To create an interface VPC endpoint for this service in your VPC, follow the [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) procedure\. For **Service Name**, choose **com\.amazonaws\.*region*\.elasticbeanstalk\-health**\.

**Important**  
The `healthd` daemon sends enhanced health information to the public endpoint, `elasticbeanstalk-health.region.amazonaws.com`\. If your VPC is configured with public internet access, and **Enable DNS name** is disabled for the VPC endpoint, enhanced health information travels through the public internet\. This is probably not your intention when you set up an enhanced health VPC endpoint\. Ensure that **Enable DNS name** is enabled \(true by default\)\.

## Using VPC endpoints in a private VPC<a name="vpc-vpce.private"></a>

A private VPC, or a private subnet in a VPC, has no public internet access\. You might want to run your Elastic Beanstalk environment in a [private VPC](vpc.md#services-vpc-private) and configure interface VPC endpoints for enhanced security\. In this case, be aware that your environment might try to connect to the internet for other reasons in addition to contacting the Elastic Beanstalk service\. To learn more about running an environment in a private VPC, see [Running an Elastic Beanstalk environment in a private VPC](vpc.md#services-vpc-private-beanstalk)\.

## Using endpoint policies to control access with VPC endpoints<a name="vpc-vpce.policy"></a>

By default, a VPC endpoint allows full access to the service with which it's associated\. When you create or modify an endpoint, you can attach an *endpoint policy* to it\. 

An endpoint policy is an AWS Identity and Access Management \(IAM\) resource policy that controls access from the endpoint to the specified service\. The endpoint policy is specific to the endpoint\. It's separate from any user or instance IAM policies that your environment might have and doesn't override or replace them\. For details about authoring and using VPC endpoint policies, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

The following example denies all users the permission to terminate an environment through the VPC endpoint, and allows full access to all other actions\.

```
{
    "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        },
        {
            "Action": "elasticbeanstalk:TerminateEnvironment",
            "Effect": "Deny",
            "Resource": "*",
            "Principal": "*"
        }
    ]
}
```

**Note**  
At this time, only the main Elastic Beanstalk service supports attaching an endpoint policy to its VPC endpoint\. The enhanced health service doesn't support endpoint policies\.