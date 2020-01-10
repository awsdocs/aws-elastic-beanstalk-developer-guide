# Managing Environments<a name="using-features.managing"></a>

AWS Elastic Beanstalk makes it easy to create new environments for your application\. You can create and manage separate environments for development, testing, and production use, and you can [deploy any version](using-features.deploy-existing-version.md) of your application to any environment\. Environments can be long\-running or temporary\. When you terminate an environment, you can save its configuration to recreate it later\.

As you develop your application, you will deploy it often, possibly to several different environments for different purposes\. Elastic Beanstalk lets you [configure how deployments are performed](using-features.rolling-version-deploy.md)\. You can deploy to all of the instances in your environment simultaneously, or split a deployment into batches with rolling deployments\.

[Configuration changes](environments-updating.md) are processed separately from deployments, and have their own scope\. For example, if you change the type of the EC2 instances running your application, all of the instances must be replaced\. On the other hand, if you modify the configuration of the environment's load balancer, that change can be made in\-place without interrupting service or lowering capacity\. You can also apply configuration changes that modify the instances in your environment in batches with [rolling configuration updates](using-features.rollingupdates.md)\.

**Note**  
Modify the resources in your environment only by using Elastic Beanstalk\. If you modify resources using another service's console, CLI commands, or SDKs, Elastic Beanstalk won't be able to accurately monitor the state of those resources, and you won't be able to save the configuration or reliably recreate the environment\. Out\-of band\-changes can also cause issues when terminating an environment\.

When you launch an environment, you choose a platform version\. We update platforms periodically with new platform versions to provide performance improvements and new features\. You can [update your environment to the latest platform version](using-features.platform.upgrade.md) at any time\.

As your application grows in complexity, you can split it into multiple components, each running in a separate environment\. For long\-running workloads, you can launch [worker environments](using-features-managing-env-tiers.md) that process jobs from an Amazon Simple Queue Service \(Amazon SQS\) queue\.

**Topics**
+ [Using the Elastic Beanstalk Environment Management Console](environments-console.md)
+ [Creating an Elastic Beanstalk Environment](using-features.environments.md)
+ [Deploying Applications to Elastic Beanstalk Environments](using-features.deploy-existing-version.md)
+ [Configuration Changes](environments-updating.md)
+ [Updating Your Elastic Beanstalk Environment's Platform Version](using-features.platform.upgrade.md)
+ [Canceling Environment Configuration Updates and Application Deployments](using-features.rollingupdates.cancel.md)
+ [Rebuilding Elastic Beanstalk Environments](environment-management-rebuild.md)
+ [Environment Types](using-features-managing-env-types.md)
+ [Elastic Beanstalk Worker Environments](using-features-managing-env-tiers.md)
+ [Creating Links Between Elastic Beanstalk Environments](environment-cfg-links.md)