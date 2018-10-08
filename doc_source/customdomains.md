# Your Elastic Beanstalk Environment's Domain Name<a name="customdomains"></a>

Your environment is available to users at a subdomain of elasticbeanstalk\.com\. When you [create an environment](using-features.environments.md), you can choose a unique subdomain that represents your application\. To route users to your environment, Elastic Beanstalk registers a CNAME record that points to your environment's load balancer\. You can see the current value of the CNAME in the [environment Dashboard](environments-console.md#environments-dashboard), as shown\.

![\[Environment URL with CNAME showing on the Elastic Beanstalk console's environment dashboard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-dashboard-url.png)

You can change the CNAME on your environment by swapping it with the CNAME of another environment\. For instructions, see [Blue/Green Deployments with AWS Elastic Beanstalk](using-features.CNAMESwap.md)\.

If you own a domain name, you can use Route 53 to resolve it to your environment\. You can purchase a domain name with Amazon Route 53, or use one that you purchase from another provider\. To purchase a domain name with Route 53, see [Registering a New Domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html) in the *Route 53 Developer Guide*\.

To use a custom domain name, first create a hosted zone for your domain\. A hosted zone contains the name server and start of authority \(SOA\) records that specify the DNS hosts that will resolve requests for your domain name\.

**To create a hosted zone in Route 53**

1. Open the [Route 53 console](https://console.aws.amazon.com/route53/home)\.

1. If you get to the Route 53 console's landing page shown in the following image, choose **Get started now** under **DNS management**\.  
![\[Route 53 console's landing page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-route53-console-landing.png)

1. Choose **Hosted Zones**\.

1. Choose **Create Hosted Zone**\.

1. For **Domain Name**, type the domain name that you own\. For example: **example\.com**\.

1. Choose **Create**\.

Next, add a record to the hosted zone that resolves your domain name to your environment\. When an Route 53 DNS server receives a name request for your custom domain name, it resolves to the elasticbeanstalk\.com subdomain, which resolves to the public DNS name of your Elastic Load Balancing load balancer, which relays requests to the instances in your environment\.

**Note**  
In a single\-instance environment, the elasticbeanstalk\.com subdomain resolves to an Elastic IP address attached to the instance running your application\.

If your environment has a regionalized subdomain, you can use an Route 53 [alias resource record set](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html) to save money on name resolution\. The domain name for an environment with a regionalized subdomain includes the region; for example, `my-environment.us-west-2.elasticbeanstalk.com`\.

**To add an alias resource record set in Route 53**

1. Open the [Route 53 console](https://console.aws.amazon.com/route53/home)\.

1. Choose **Hosted Zones**\.

1. Choose your hosted zone's name\.

1. Choose **Create Record Set**\.

1. For **Name**, type the subdomain that will redirect to your Elastic Beanstalk application\. For example: **www**\.

1. For **Type**, choose **A \- IPv4 address**\.

1. For **Alias**, choose **yes**\.

1. For **Alias Target**, choose the domain name of your Elastic Beanstalk environment\.

1. Choose **Save Record Set**\.

If your environment doesn't have a regionalized subdomain, create a CNAME record instead\.

**To add a CNAME record in Route 53**

1. Open the [Route 53 console](https://console.aws.amazon.com/route53/home)\.

1. Choose **Hosted Zones**\.

1. Choose your hosted zone's name\.

1. Choose **Create Record Set**\.

1. For **Name**, type the subdomain that will redirect to your Elastic Beanstalk application\. For example: **www**\.

1. For **Type**, choose **CNAME \- Canonical Name**\.

1. For **Value**, type the domain name of your Elastic Beanstalk environment\. For example: **example\.elasticbeanstalk\.com**\.

1. Choose **Save Record Set**\.

DNS records take up to 24 hours to propagate worldwide\.

If you registered a domain name with another provider, register the name servers in your Route 53 hosted zone in the domain configuration\. When your provider receives DNS requests for your domain name, it will forward them to the Route 53 name servers to resolve the domain name to an IP address\. Look for a setting called *Nameservers*, or check your provider's documentation\.

The Route 53 console displays the list of name servers for your hosted zone in an NS record on the **Hosted Zones** page\.

![\[Route 53 list of name servers for your hosted zone\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/route53-nameservers.png)

If you have multiple environments running your application, you can use the Elastic Beanstalk console to swap the domain names of two environments\. This allows you to deploy a new version of your application to a standby environment, test it, and then swap domains with the production environment\.

When you perform a CNAME swap, users are directed to the new version of your application with zero downtime\. This is known as a *blue/green deployment*\.

**To swap environment CNAMEs**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose either environment to open the environment dashboard\.

1. Choose **Actions**, and then choose **Swap Environment URLs**\.

1. Select the other environment, and then choose **Swap**\.