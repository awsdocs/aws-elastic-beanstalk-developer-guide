# Blue/Green deployments with Elastic Beanstalk<a name="using-features.CNAMESwap"></a>

Because AWS Elastic Beanstalk performs an in\-place update when you update your application versions, your application might become unavailable to users for a short period of time\. To avoid this, perform a blue/green deployment\. To do this, deploy the new version to a separate environment, and then swap the CNAMEs of the two environments to redirect traffic to the new version instantly\.

A blue/green deployment is also required if you want to update an environment to an incompatible platform version\. For more information, see [Updating your Elastic Beanstalk environment's platform version](using-features.platform.upgrade.md)\.

Blue/green deployments require that your environment runs independently of your production database, if your application uses one\. If your environment includes a database that Elastic Beanstalk created on your behalf, the database and connection of the environment isn't preserved unless you take specific actions\. If you have a database that you want to retain, use one of the Elastic Beanstalk database lifecycle options\. You can choose the Retain option to keep the database and environment operational after decoupling the database\. For more information see [Database lifecycle](using-features.managing.db.md#environments-cfg-rds-lifecycle) in the *Configuring environments* chapter of this guide\. 

For instructions on how to configure your application to connect to an Amazon RDS instance that's not managed by Elastic Beanstalk, see [Using Elastic Beanstalk with Amazon RDS](AWSHowTo.RDS.md)\.

**To perform a blue/green deployment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. [Clone your current environment](using-features.managing.clone.md), or launch a new environment to run the platform version you want\.

1. [Deploy the new application version](using-features.deploy-existing-version.md#deployments-newversion) to the new environment\.

1. Test the new version on the new environment\.

1. On the environment overview page, choose **Environment actions**, and then choose **Swap environment URLs**\.

1. For **Environment name**, select the current environment\.  
![\[Swap environment URL page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-swap-url.png)

1. Choose **Swap**\.

Elastic Beanstalk swaps the CNAME records of the old and new environments, redirecting traffic from the old version to the new version and vice versa\.

After Elastic Beanstalk completes the swap operation, verify that the new environment responds when you try to connect to the old environment URL\. However, do not terminate your old environment until the DNS changes are propagated and your old DNS records expire\. DNS servers don't always clear old records from their cache based on the time to live \(TTL\) that you set on your DNS records\.