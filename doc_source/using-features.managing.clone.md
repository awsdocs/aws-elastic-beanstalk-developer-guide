# Clone an Environment<a name="using-features.managing.clone"></a>

You can use an existing environment as the basis for a new environment by creating a clone of the existing environment\. For example, you might want to create a clone so that you can use a newer version of the solution stack used by the original environment's platform\. Elastic Beanstalk configures the clone with the same environment settings used by the original environment\. By cloning an existing environment instead of creating a new environment, you do not have to manually configure option settings, environment variables, and other settings\. Elastic Beanstalk also creates a copy of any AWS resource associated with the original environment\. However, during the cloning process, Elastic Beanstalk does not copy data from Amazon RDS to the clone\. After you have created the clone environment, you can modify environment configuration settings as needed\.

**Note**  
Elastic Beanstalk does not include any unmanaged changes to resources in the clone\. Changes to AWS resources that you make using tools other than the Elastic Beanstalk management console, command\-line tools, or API are considered unmanaged changes\.

## AWS Management Console<a name="using-features.managing.clone.CON"></a>

**To clone an environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. From the region list, select the region that includes the environment that you want to work with\.

1. On the Elastic Beanstalk console applications page, click the name of the application, and then the name of the environment that you want to clone\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-page-env.png)

1. On the environment dashboard, click **Actions**, and then do one of the following: 

   + Click **Clone Environment** if you want to clone the environment without any changes to the solution stack version\.

   + Click **Clone with Latest Platform** if you want to clone the environment, but with a newer version of the original environment's solution stack\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-dashboard-action.png)

1. On the **Clone Environment** page, review the information in the **Original Environment** section to verify that you chose the environment from which you want to create a clone\.

1. In the **New Environment** section, you can optionally change the **Environment name**, **Environment URL**, **Description**, and **Platform** values that Elastic Beanstalk automatically set based on the original environment\.
**Note**  
For **Platform**, only solution stacks with the same language and web server configuration are shown\. If a newer version of the solution stack used with the original environment is available, you will be prompted to update, but you cannot choose a different stack, even if it is for a different version of the same language\. For more information, see [Elastic Beanstalk Supported Platforms](concepts.platforms.md)\.  
![\[Elastic Beanstalk Clone Environment Configuration Window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-clone-env-latest.png)

1. When you are ready, click **Clone**\.

## Elastic Beanstalk Command Line Interface \(EB CLI\)<a name="using-features.managing.clone.CLI"></a>

Use the `eb clone` command to clone a running environment:

```
~/workspace/my-app$ eb clone my-env1
Enter name for Environment Clone
(default is my-env1-clone): my-env2
Enter DNS CNAME prefix
(default is my-env1-clone): my-env2
```

You can specify the name of the source environment in the clone command, or leave it out to clone the default environment for the current project folder\. The EB CLI prompts you to enter a name and DNS prefix for the new environment\.

By default, `eb clone` creates the new environment with the latest available version of the source environment's platform\. To force the EB CLI to use the same version, even if there is a newer version available, use the `--exact` option:

```
~/workspace/my-app$ eb clone --exact
```

For more information about this command, see eb clone\.