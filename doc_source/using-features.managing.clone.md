# Clone an Elastic Beanstalk environment<a name="using-features.managing.clone"></a>

You can use an existing Elastic Beanstalk environment as the basis for a new environment by cloning the existing environment\. For example, you might want to create a clone so that you can use a newer version of the platform branch used by the original environment's platform\. Elastic Beanstalk configures the clone with the same environment settings used by the original environment\. By cloning an existing environment instead of creating a new environment, you don't have to manually configure option settings, environment variables, and other settings\. Elastic Beanstalk also creates a copy of any AWS resource associated with the original environment\. However, during the cloning process, Elastic Beanstalk doesn't copy data from Amazon RDS to the clone\. After you create the clone environment, you can modify environment configuration settings as needed\.

You can only clone an environment to a different platform version of the same platform branch\. A different platform branch isn't guaranteed to be compatible\. To use a different platform branch, you have to manually create a new environment, deploy your application code, and make any necessary changes in code and options to ensure your application works correctly on the new platform branch\.

**Note**  
Elastic Beanstalk doesn't include any unmanaged changes to resources in the clone\. Changes to AWS resources that you make using tools other than the Elastic Beanstalk console, command\-line tools, or API are considered unmanaged changes\.

## AWS management console<a name="using-features.managing.clone.CON"></a>

**To clone an environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Environment actions**, and then do one of the following: 
   + Choose **Clone environment** to clone the environment without any changes to the platform version\.
   + Choose **Clone with latest platform** to clone the environment, but with a newer version of the original environment's platform branch\.

1. On the **Clone environment** page, review the information in the **Original Environment** section to verify that you chose the environment from which you want to create a clone\.

1. In the **New Environment** section, you can optionally change the **Environment name**, **Environment URL**, **Description**, **Platform version**, and **Service role** values that Elastic Beanstalk automatically set based on the original environment\.
**Note**  
If the platform version used in the original environment isn't the one recommended for use in the platform branch, you are warned that a different platform version is recommended\. Choose **Platform version**, and you can see the recommended platform version on the list—for example, **3\.3\.2 \(Recommended\)**\.  
![\[Elastic Beanstalk clone environment configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-clone-env-latest.png)

1. When you are ready, choose **Clone**\.

## Elastic Beanstalk command line interface \(EB CLI\)<a name="using-features.managing.clone.CLI"></a>

Use the eb clone command to clone a running environment, as follows\.

```
~/workspace/my-app$ eb clone my-env1
Enter name for Environment Clone
(default is my-env1-clone): my-env2
Enter DNS CNAME prefix
(default is my-env1-clone): my-env2
```

You can specify the name of the source environment in the clone command, or leave it out to clone the default environment for the current project folder\. The EB CLI prompts you to enter a name and DNS prefix for the new environment\.

By default, eb clone creates the new environment with the latest available version of the source environment's platform\. To force the EB CLI to use the same version, even if there is a newer version available, use the `--exact` option\.

```
~/workspace/my-app$ eb clone --exact
```

For more information about this command, see [eb clone](eb3-clone.md)\.