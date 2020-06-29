# Deploying applications to Elastic Beanstalk environments<a name="using-features.deploy-existing-version"></a>

You can use the AWS Elastic Beanstalk console to upload an updated [source bundle](applications-sourcebundle.md) and deploy it to your Elastic Beanstalk environment, or redeploy a previously uploaded version\.

Each deployment is identified by a deployment ID\. Deployment IDs start at `1` and increment by one with each deployment and instance configuration change\. If you enable [enhanced health reporting](health-enhanced.md), Elastic Beanstalk displays the deployment ID in both the [health console](health-enhanced-console.md) and the [EB CLI](health-enhanced-ebcli.md) when it reports instance health status\. The deployment ID helps you determine the state of your environment when a rolling update fails\.

Elastic Beanstalk provides several deployment policies and settings\. For details about configuring a policy and additional settings, see [Deployment policies and settings](using-features.rolling-version-deploy.md)\. The following table lists the policies and the kinds of environments that support them\.


**Supported deployment policies**  

| Deployment policy | Load\-balanced environments | Single\-instance environments | Legacy Windows Server environments† | 
| --- | --- | --- | --- | 
|  **All at once**  |   ✓ Yes  |   ✓ Yes  |   ✓ Yes  | 
|  **Rolling**  |   ✓ Yes  |   ☓ No  |   ✓ Yes  | 
|  **Rolling with an additional batch**  |   ✓ Yes  |   ☓ No  |   ☓ No  | 
|  **Immutable**  |   ✓ Yes  |   ✓ Yes  |   ☓ No  | 
|  **Traffic splitting**  |   ✓ Yes \(Application Load Balancer\)  |   ☓ No  |   ☓ No  | 

† In this table, a *Legacy Windows Server environment* is an environment based on a [Windows Server platform configuration](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net) that uses an IIS version earlier than IIS 8\.5\.

**Warning**  
Some policies replace all instances during the deployment or update\. This causes all accumulated [Amazon EC2 burst balances](https://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/burstable-performance-instances.html) to be lost\. It happens in the following cases:  
Managed platform updates with instance replacement enabled
Immutable updates
Deployments with immutable updates or traffic splitting enabled

## Choosing a deployment policy<a name="deployments-scenarios"></a>

Choosing the right deployment policy for your application is a tradeoff of a few considerations, and depends on your particular needs\. The [Deployment policies and settings](using-features.rolling-version-deploy.md) page has more information about each policy, and a detailed description of the workings of some of them\.

The following list provides summary information about the different deployment policies and adds related considerations\.
+ **All at once** – The quickest deployment method\. Suitable if you can accept a short loss of service, and if quick deployments are important to you\. With this method, Elastic Beanstalk deploys the new application version to each instance\. Then, the web proxy or application server might need to restart\. As a result, your application might be unavailable to users \(or have low availability\) for a short time\.
+ **Rolling** – Avoids downtime and minimizes reduced availability, at a cost of a longer deployment time\. Suitable if you can't accept any period of completely lost service\. With this method, your application is deployed to your environment one batch of instances at a time\. Most bandwidth is retained throughout the deployment\.
+ **Rolling with additional batch** – Avoids any reduced availability, at a cost of an even longer deployment time compared to the *Rolling* method\. Suitable if you must maintain the same bandwidth throughout the deployment\. With this method, Elastic Beanstalk launches an extra batch of instances, then performs a rolling deployment\. Launching the extra batch takes time, and ensures that the same bandwidth is retained throughout the deployment\.
+ **Immutable** – A slower deployment method, that ensures your new application version is always deployed to new instances, instead of updating existing instances\. It also has the additional advantage of a quick and safe rollback in case the deployment fails\. With this method, Elastic Beanstalk performs an [immutable update](environmentmgmt-updates-immutable.md) to deploy your application\. In an immutable update, a second Auto Scaling group is launched in your environment and the new version serves traffic alongside the old version until the new instances pass health checks\.
+ **Traffic splitting** – A canary testing deployment method\. Suitable if you want to test the health of your new application version using a portion of incoming traffic, while keeping the rest of the traffic served by the old application version\. 

The following table compares deployment method properties\.


**Deployment methods**  

| **Method** | **Impact of failed deployment** | **Deploy time** | **Zero downtime** | **No DNS change** | **Rollback process** | **Code deployed to** | 
| --- | --- | --- | --- | --- | --- | --- | 
| All at once | Downtime | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png) |  ☓ No |  ✓ Yes | Manual redeploy | Existing instances | 
| Rolling | Single batch out of service; any successful batches before failure running new application version | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)† |  ✓ Yes |  ✓ Yes | Manual redeploy | Existing instances | 
| Rolling with an additional batch | Minimal if first batch fails; otherwise, similar to Rolling | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)† |  ✓ Yes |  ✓ Yes | Manual redeploy | New and existing instances | 
| Immutable | Minimal | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png) |  ✓ Yes |  ✓ Yes | Terminate new instances | New instances | 
| Traffic splitting | Percentage of client traffic routed to new version temporarily impacted | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)†† |  ✓ Yes |  ✓ Yes | Reroute traffic and terminate new instances | New instances | 
| Blue/green | Minimal | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clock.png) |  ✓ Yes |  ☓ No | Swap URL | New instances | 

† *Varies depending on batch size\.*

†† *Varies depending on **evaluation time** option setting\.*

## Deploying a new application version<a name="deployments-newversion"></a>

You can perform deployments from your environment's dashboard\.

**To deploy a new application version to an Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Upload and deploy**\.

1. Use the on\-screen form to upload the application source bundle\.

1. Choose **Deploy**\.

## Redeploying a previous version<a name="deployments-existingversion"></a>

You can also deploy a previously uploaded version of your application to any of its environments from the application versions page\. 

**To deploy an existing application version to an existing environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose your application's name from the list\.
**Note**  
If you have many applications, use the search bar to filter the application list\.

1. In the navigation pane, find your application's name and choose **Application versions**\.

1. Select the application version to deploy\.

1. Choose **Actions**, and then choose **Deploy**\.

1. Select an environment, and then choose **Deploy**\.

## Other ways to deploy your application<a name="deployments-other"></a>

If you deploy often, consider using the [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) to manage your environments\. The EB CLI creates a repository alongside your source code\. It can also create a source bundle, upload it to Elastic Beanstalk, and deploy it with a single command\.

For deployments that depend on resource configuration changes or a new version that can't run alongside the old version, you can launch a new environment with the new version and perform a CNAME swap for a [blue/green deployment](using-features.CNAMESwap.md)\.