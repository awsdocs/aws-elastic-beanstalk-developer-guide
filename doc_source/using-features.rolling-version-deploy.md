# Deployment policies and settings<a name="using-features.rolling-version-deploy"></a>

AWS Elastic Beanstalk provides several options for how [deployments](using-features.deploy-existing-version.md) are processed, including deployment policies \(*All at once*, *Rolling*, *Rolling with additional batch*, *Immutable*, and *Traffic splitting*\) and options that let you configure batch size and health check behavior during deployments\. By default, your environment uses all\-at\-once deployments\. If you created the environment with the EB CLI and it's a scalable environment \(you didn't specify the `--single` option\), it uses rolling deployments\.

With *rolling deployments*, Elastic Beanstalk splits the environment's Amazon EC2 instances into batches and deploys the new version of the application to one batch at a time\. It leaves the rest of the instances in the environment running the old version of the application\. During a rolling deployment, some instances serve requests with the old version of the application, while instances in completed batches serve other requests with the new version\. For details, see [How rolling deployments work](#environments-cfg-rollingdeployments-method)\.

To maintain full capacity during deployments, you can configure your environment to launch a new batch of instances before taking any instances out of service\. This option is known as a *rolling deployment with an additional batch*\. When the deployment completes, Elastic Beanstalk terminates the additional batch of instances\.

*Immutable deployments* perform an [immutable update](environmentmgmt-updates-immutable.md) to launch a full set of new instances running the new version of the application in a separate Auto Scaling group, alongside the instances running the old version\. Immutable deployments can prevent issues caused by partially completed rolling deployments\. If the new instances don't pass health checks, Elastic Beanstalk terminates them, leaving the original instances untouched\.

*Traffic\-splitting deployments* let you perform canary testing as part of your application deployment\. In a traffic\-splitting deployment, Elastic Beanstalk launches a full set of new instances just like during an immutable deployment\. It then forwards a specified percentage of incoming client traffic to the new application version for a specified evaluation period\. If the new instances stay healthy, Elastic Beanstalk forwards all traffic to them and terminates the old ones\. If the new instances don't pass health checks, or if you choose to abort the deployment, Elastic Beanstalk moves traffic back to the old instances and terminates the new ones\. There's never any service interruption\. For details, see [How traffic\-splitting deployments work](#environments-cfg-trafficsplitting-method)\.

**Warning**  
Some policies replace all instances during the deployment or update\. This causes all accumulated [Amazon EC2 burst balances](https://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/burstable-performance-instances.html) to be lost\. It happens in the following cases:  
Managed platform updates with instance replacement enabled
Immutable updates
Deployments with immutable updates or traffic splitting enabled

If your application doesn't pass all health checks, but still operates correctly at a lower health status, you can allow instances to pass health checks with a lower status, such as `Warning`, by modifying the **Healthy threshold** option\. If your deployments fail because they don't pass health checks and you need to force an update regardless of health status, specify the **Ignore health check** option\.

When you specify a batch size for rolling updates, Elastic Beanstalk also uses that value for rolling application restarts\. Use rolling restarts when you need to restart the proxy and application servers running on your environment's instances without downtime\.

## Configuring application deployments<a name="environments-cfg-rollingdeployments-console"></a>

In the [environment management console](environments-console.md), enable and configure batched application version deployments by editing **Updates and Deployments** on the environment's **Configuration** page\.

**To configure deployments \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Rolling updates and deployments** configuration category, choose **Edit**\.

1. In the **Application Deployments** section, choose a **Deployment policy**, batch settings, and health check options\.

1. Choose **Apply**\.

The **Application deployments** section of the **Rolling updates and deployments** page has the following options for application deployments:
+ **Deployment policy** – Choose from the following deployment options:
  + **All at once** – Deploy the new version to all instances simultaneously\. All instances in your environment are out of service for a short time while the deployment occurs\.
  + **Rolling** – Deploy the new version in batches\. Each batch is taken out of service during the deployment phase, reducing your environment's capacity by the number of instances in a batch\.
  + **Rolling with additional batch** – Deploy the new version in batches, but first launch a new batch of instances to ensure full capacity during the deployment process\.
  + **Immutable** – Deploy the new version to a fresh group of instances by performing an [immutable update](environmentmgmt-updates-immutable.md)\.
  + **Traffic splitting** – Deploy the new version to a fresh group of instances and temporarily split incoming client traffic between the existing application version and the new one\.

For the **Rolling** and **Rolling with additional batch** deployment policies you can configure:
+ **Batch size** – The size of the set of instances to deploy in each batch\.

  Choose **Percentage** to configure a percentage of the total number of EC2 instances in the Auto Scaling group \(up to 100 percent\), or choose **Fixed** to configure a fixed number of instances \(up to the maximum instance count in your environment's Auto Scaling configuration\)\.

For the **Traffic splitting** deployment policy you can configure the following:
+ **Traffic split** – The initial percentage of incoming client traffic that Elastic Beanstalk shifts to environment instances running the new application version you're deploying\.
+ **Traffic splitting evaluation time** – The time period, in minutes, that Elastic Beanstalk waits after an initial healthy deployment before proceeding to shift all incoming client traffic to the new application version that you're deploying\.

![\[Elastic Beanstalk application deployment configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-rollingdeployments.png)

The **Deployment preferences** section contains options related to health checks\.
+ **Ignore health check** – Prevents a deployment from rolling back when a batch fails to become healthy within the **Command timeout**\.
+ **Healthy threshold** – Lowers the threshold at which an instance is considered healthy during rolling deployments, rolling updates, and immutable updates\.
+ **Command timeout** – The number of seconds to wait for an instance to become healthy before canceling the deployment or, if **Ignore health check** is set, to continue to the next batch\.

![\[Elastic Beanstalk application deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-healthchecks.png)

## How rolling deployments work<a name="environments-cfg-rollingdeployments-method"></a>

When processing a batch, Elastic Beanstalk detaches all instances in the batch from the load balancer, deploys the new application version, and then reattaches the instances\. If you enable [connection draining](environments-cfg-clb.md#using-features.managing.elb.draining), Elastic Beanstalk drains existing connections from the Amazon EC2 instances in each batch before beginning the deployment\.

After reattaching the instances in a batch to the load balancer, Elastic Load Balancing waits until they pass a minimum number of Elastic Load Balancing health checks \(the **Healthy check count threshold** value\), and then starts routing traffic to them\. If no [health check URL](environments-cfg-clb.md#using-features.managing.elb.healthchecks) is configured, this can happen very quickly, because an instance will pass the health check as soon as it can accept a TCP connection\. If a health check URL is configured, the load balancer doesn't route traffic to the updated instances until they return a `200 OK` status code in response to an `HTTP GET` request to the health check URL\.

Elastic Beanstalk waits until all instances in a batch are healthy before moving on to the next batch\. With [basic health reporting](using-features.healthstatus.md), instance health depends on the Elastic Load Balancing health check status\. When all instances in the batch pass enough health checks to be considered healthy by Elastic Load Balancing, the batch is complete\. If [enhanced health reporting](health-enhanced.md) is enabled, Elastic Beanstalk considers several other factors, including the result of incoming requests\. With enhanced health reporting, all instances must pass 12 consecutive health checks with an [OK status](health-enhanced-status.md#health-enhanced-status-ok) within two minutes for web server environments, and 18 health checks within three minutes for worker environments\.

If a batch of instances does not become healthy within the [command timeout](#environments-cfg-rollingdeployments-console), the deployment fails\. After a failed deployment, [check the health of the instances in your environment](health-enhanced-console.md) for information about the cause of the failure\. Then perform another deployment with a fixed or known good version of your application to roll back\.

If a deployment fails after one or more batches completed successfully, the completed batches run the new version of your application while any pending batches continue to run the old version\. You can identify the version running on the instances in your environment on the [health page](health-enhanced-console.md#health-enhanced-console-healthpage) in the console\. This page displays the deployment ID of the most recent deployment that executed on each instance in your environment\. If you terminate instances from the failed deployment, Elastic Beanstalk replaces them with instances running the application version from the most recent successful deployment\.

## How traffic\-splitting deployments work<a name="environments-cfg-trafficsplitting-method"></a>

Traffic\-splitting deployments allow you to perform canary testing\. You direct some incoming client traffic to your new application version to verify the application's health before committing to the new version and directing all traffic to it\.

During a traffic\-splitting deployment, Elastic Beanstalk creates a new set of instances in a separate temporary Auto Scaling group\. Elastic Beanstalk then instructs the load balancer to direct a certain percentage of your environment's incoming traffic to the new instances\. Then, for a configured amount of time, Elastic Beanstalk tracks the health of the new set of instances\. If all is well, Elastic Beanstalk shifts remaining traffic to the new instances and attaches them to the environment's original Auto Scaling group, replacing the old instances\. Then Elastic Beanstalk cleans up—terminates the old instances and removes the temporary Auto Scaling group\.

**Note**  
The environment's capacity doesn't change during a traffic\-splitting deployment\. Elastic Beanstalk launches the same number of instances in the temporary Auto Scaling group as there are in the original Auto Scaling group at the time the deployment starts\. It then maintains a constant number of instances in both Auto Scaling groups for the deployment duration\. Take this fact into account when configuring the environment's traffic splitting evaluation time\.

Rolling back the deployment to the previous application version is quick and doesn't impact service to client traffic\. If the new instances don't pass health checks, or if you choose to abort the deployment, Elastic Beanstalk moves traffic back to the old instances and terminates the new ones\. You can abort any deployment by using the environment overview page in the Elastic Beanstalk console, and choosing **Abort current operation** in **Environment actions**\. You can also call the [AbortEnvironmentUpdate](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_AbortEnvironmentUpdate.html) API or the equivalent AWS CLI command\.

Traffic\-splitting deployments require an Application Load Balancer\. Elastic Beanstalk uses this load balancer type by default when you create your environment using the Elastic Beanstalk console or the EB CLI\.

## Deployment option namespaces<a name="environments-cfg-rollingdeployments-namespace"></a>

You can use the [configuration options](command-options.md) in the [`aws:elasticbeanstalk:command`](command-options-general.md#command-options-general-elasticbeanstalkcommand) namespace to configure your deployments\. If you choose the traffic\-splitting policy, additional options for this policy are available in the [`aws:elasticbeanstalk:trafficsplitting`](command-options-general.md#command-options-general-elasticbeanstalktrafficsplitting) namespace\.

Use the `DeploymentPolicy` option to set the deployment type\. The following values are supported:
+ `AllAtOnce` – Disables rolling deployments and always deploys to all instances simultaneously\.
+ `Rolling` – Enables standard rolling deployments\.
+ `RollingWithAdditionalBatch` – Launches an extra batch of instances, before starting the deployment, to maintain full capacity\.
+ `Immutable` – Performs an [immutable update](environmentmgmt-updates-immutable.md) for every deployment\.
+ `TrafficSplitting` – Performs traffic\-splitting deployments to canary\-test your application deployments\.

When you enable rolling deployments, set the `BatchSize` and `BatchSizeType` options to configure the size of each batch\. For example, to deploy 25 percent of all instances in each batch, specify the following options and values\.

**Example \.ebextensions/rolling\-updates\.config**  

```
option_settings:
  aws:elasticbeanstalk:command:
    DeploymentPolicy: Rolling
    BatchSizeType: Percentage
    BatchSize: 25
```

To deploy to five instances in each batch, regardless of the number of instances running, and to bring up an extra batch of five instances running the new version before pulling any instances out of service, specify the following options and values\.

**Example \.ebextensions/rolling\-additionalbatch\.config**  

```
option_settings:
  aws:elasticbeanstalk:command:
    DeploymentPolicy: RollingWithAdditionalBatch
    BatchSizeType: Fixed
    BatchSize: 5
```

To perform an immutable update for each deployment with a health check threshold of **Warning**, and proceed with the deployment even if instances in a batch don't pass health checks within a timeout of 15 minutes, specify the following options and values\.

**Example \.ebextensions/immutable\-ignorehealth\.config**  

```
option_settings:
  aws:elasticbeanstalk:command:
    DeploymentPolicy: Immutable
    HealthCheckSuccessThreshold: Warning
    IgnoreHealthCheck: true
    Timeout: "900"
```

To perform traffic\-splitting deployments, forwarding 15 percent of client traffic to the new application version and evaluating health for 10 minutes, specify the following options and values\.

**Example \.ebextensions/traffic\-splitting\.config**  

```
option_settings:
  aws:elasticbeanstalk:command:
    DeploymentPolicy: TrafficSplitting
  aws:elasticbeanstalk:trafficsplitting:
    NewVersionPercent: "15"
    EvaluationTime: "10"
```

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended values](command-options.md#configuration-options-recommendedvalues) for details\.