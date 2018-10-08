# Configuring HTTPS for your Elastic Beanstalk Environment<a name="configuring-https"></a>

If you've purchased and configured a [custom domain name](customdomains.md) for your Elastic Beanstalk environment, you can use HTTPS to allow users to connect to your web site securely\. If you don't own a domain name, you can still use HTTPS with a self\-signed certificate for development and testing purposes\. HTTPS is a must for any application that transmits user data or login information\.

The simplest way to use HTTPS with an Elastic Beanstalk environment is to [assign a server certificate to your environment's load balancer](configuring-https-elb.md)\. When you configure your load balancer to terminate HTTPS, the connection between the client and the load balancer is secure\. Backend connections between the load balancer and EC2 instances use HTTP, so no additional configuration of the instances is required\.

**Note**  
With [AWS Certificate Manager \(ACM\)](https://aws.amazon.com/certificate-manager/), you can create a trusted certificate for your domain names for free\. ACM certificates can only be used with AWS load balancers and Amazon CloudFront distributions, and ACM is [available only in certain regions](http://docs.aws.amazon.com/general/latest/gr/rande.html#acm_region)\.  
To use an ACM certificate with Elastic Beanstalk, see [Configuring Your Elastic Beanstalk Environment's Load Balancer to Terminate HTTPS](configuring-https-elb.md)\.

If you run your application in a single instance environment, or need to secure the connection all the way to the EC2 instances behind the load balancer, you can [configure the proxy server that runs on the instance to terminate HTTPS](https-singleinstance.md)\. Configuring your instances to terminate HTTPS connections requires the use of [configuration files](ebextensions.md) to modify the software running on the instances, and to modify security groups to allow secure connections\.

For end\-to\-end HTTPS in a load balanced environment, you can [combine instance and load balancer termination](configuring-https-endtoend.md) to encrypt both connections\. By default, if you configure the load balancer to forward traffic using HTTPS, it will trust any certificate presented to it by the backend instances\. For maximum security, you can attach policies to the load balancer that prevent it from connecting to instances that don't present a public certificate that it trusts\.

**Note**  
You can also configure the load balancer to [relay HTTPS traffic without decrypting it](https-tcp-passthrough.md)\. The down side to this method is that the load balancer cannot see the requests and thus cannot optimize routing or report response metrics\.

If ACM is not available in your region, you can purchase a trusted certificate from a third party\. A third\-party certificate can be used to decrypt HTTPS traffic at your load balancer, on the backend instances, or both\.

For development and testing, you can [create and sign a certificate](configuring-https-ssl.md) yourself with open source tools\. Self\-signed certificates are free and easy to create, but cannot be used for front\-end decryption on public sites\. If you attempt to use a self\-signed certificate for an HTTPS connection to a client, the user's browser displays an error message indicating that your web site is unsafe\. You can, however, use a self\-signed certificate to secure backend connections without issue\.

ACM is the preferred tool to provision, manage, and deploy your server certificates programmatically or using the AWS CLI\. If ACM is not [available in your region](http://docs.aws.amazon.com/general/latest/gr/rande.html#acm_region), you can [upload a third\-party or self\-signed certificate and private key](configuring-https-ssl-upload.md) to AWS Identity and Access Management \(IAM\) by using the AWS CLI\. Certificates stored in IAM can be used with load balancers and CloudFront distributions\.

**Note**  
The [Does it have Snakes?](https://github.com/awslabs/eb-tomcat-snakes) sample application on GitHub includes configuration files and instructions for each method of configuring HTTPS with a Tomcat web application\. See the [readme file](https://github.com/awslabs/eb-tomcat-snakes/blob/master/README.md) and [HTTPS instructions](https://github.com/awslabs/eb-tomcat-snakes/blob/master/src/.ebextensions/inactive/HTTPS.md) for details\.

**Topics**
+ [Create and Sign an X509 Certificate](configuring-https-ssl.md)
+ [Upload a Certificate to IAM](configuring-https-ssl-upload.md)
+ [Configuring Your Elastic Beanstalk Environment's Load Balancer to Terminate HTTPS](configuring-https-elb.md)
+ [Configuring Your Application to Terminate HTTPS Connections at the Instance](https-singleinstance.md)
+ [Configuring End\-to\-End Encryption in a Load Balanced Elastic Beanstalk Environment](configuring-https-endtoend.md)
+ [Configuring Your Environment's Load Balancer for TCP Passthrough](https-tcp-passthrough.md)
+ [Storing Private Keys Securely in Amazon S3](https-storingprivatekeys.md)
+ [Configuring HTTP to HTTPS Redirection](configuring-https-httpredirect.md)