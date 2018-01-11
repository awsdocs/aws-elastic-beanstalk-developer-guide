# Blue/Green Deployments with AWS Elastic Beanstalk<a name="using-features.CNAMESwap"></a>

Because Elastic Beanstalk performs an in\-place update when you update your application versions, your application may become unavailable to users for a short period of time\. It is possible to avoid this downtime by performing a blue/green deployment, where you deploy the new version to a separate environment, and then swap CNAMEs of the two environments to redirect traffic to the new version instantly\.

Blue/green deployments require that your environment runs independently of your production database, if your application uses one\. If your environment has an Amazon RDS DB instance attached to it, the data will not transfer over to your second environment, and will be lost if you terminate the original environment\.

For details on configuring your application to connect to an external \(not managed by Elastic Beanstalk\) Amazon RDS instance, see \.

**To perform a blue/green deployment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Clone your current environment, or launch a new environment running the desired configuration\.

1. Deploy the new application version to the new environment\.

1. Test the new version on the new environment\.

1. From the new environment's dashboard, choose **Actions** and then choose **Swap Environment URLs**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-dashboard-swapurls.png)

1. From the **Environment name** drop\-down list, select the current environment\.  
![\[Swap Environment URL Window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-swap-url.png)

1. Choose **Swap**\.

1. Elastic Beanstalk swaps the CNAME records of the old and new environment, redirecting traffic from the old version to the new version and vice\-versa\.  
![\[Swap Environment URL Events\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cnameswap-events.png)

After Elastic Beanstalk completes the swap operation, verify that the new environment responds when you try to connect to the old environment URL\. However, do not terminate your old environment until the DNS changes have been propagated and your old DNS records expire\. DNS servers do not necessarily clear old records from their cache based on the time to live \(TTL\) you set on your DNS records\.