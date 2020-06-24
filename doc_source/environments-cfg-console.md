# Environment configuration using the Elastic Beanstalk console<a name="environments-cfg-console"></a>

You can use the Elastic Beanstalk console to view and modify many [configuration options](command-options.md) of your environment and its resources\. You can customize how the environment behaves during deployments, enable additional features, and modify the instance type and other settings that you chose during environment creation\.

**To view a summary of your environment's configuration**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

## Configuration overview page<a name="environments-cfg-console.overview"></a>

The **Configuration overview** page shows a set of configuration categories\. Each configuration category summarizes the current state of a group of related options\.

You can choose two views for this page\. Turn on **Table View** to see a list of options grouped by category\.

![\[Table view of the configuration overview page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.table.png)

You can search for an option by its name or value by entering search terms into a search box\. As you type, the list gets shorter and shows only options that match your search terms\.

![\[Table view of the configuration overview page of the Elastic Beanstalk console, showing an option search\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.table.search1.png)![\[Table view of the configuration overview page of the Elastic Beanstalk console, showing an option search\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.table.search2.png)![\[Table view of the configuration overview page of the Elastic Beanstalk console, showing an option search\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.table.search3.png)

Turn off **Table View** to see each category in a separate frame \(configuration card\)\.

![\[Grid view of the configuration overview page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.png)

Choose **Edit** in a configuration category to get to a related configuration page, where you can see full option values and make changes\. When you're done viewing and modifying options, you can choose one of the following actions:
+ **Cancel** – Go back to the environment's dashboard without applying your configuration changes\. When you choose **Cancel**, the console loses any pending changes you made on any configuration category\.

  You can also cancel your configuration changes by choosing another console page, like **Dashboard** or **Logs**\. In this case, if there are any pending configuration changes, the console prompts you to confirm that you agree to losing them\.
+ **Review changes** – Get a summary of all the pending changes you made in any of the configuration categories\. For details, see [Review changes page](#environments-cfg-console.review)\.
+ **Apply configuration** – Apply the changes you made in any of the configuration categories to your environment\. In some cases you're prompted to confirm a consequence of one of your configuration decisions\.

## Review changes page<a name="environments-cfg-console.review"></a>

The **Review Changes** page displays a table showing all the pending option changes you made in any of the configuration categories and haven't applied to your environment yet\. If you removed any options \(for example, a custom environment property\), a second table shows the removed options\.

Both tables list each option as a combination of the **Namespace** and **Option Name** with which Elastic Beanstalk identifies it\. For details, see [Configuration options](command-options.md)\.

For example, you might make these configuration changes:
+ In the **Capacity** category: Change **Instances \(Min\)** from 1 to 2, and **Instances \(Max\)** from 2 to 4\. This change corresponds to two changes in the `aws:autoscaling:asg` namespace on the **Changed Options** list\.
+ In the **Software** category:
  + Enable the **Rotate logs** option\. This change corresponds to a change in the `aws:elasticbeanstalk:hostmanager` namespace on the **Changed Options** list\.
  + Remove the `MY_ENV_PROPERTY` environment property\. This change corresponds to a single entry for the `aws:elasticbeanstalk:application:environment` namespace on the **Removed Options** list\.
+ In the **Managed updates** category: Enable the **Managed updates** option\. This single configuration change corresponds to three option changes across two namespaces—the last three items on the **Changed Options** list\.

The following image shows the lists of your configuration changes on the **Review Changes** page\.

![\[Review changes in the configuration page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.review.png)

When you're done reviewing your changes, you can choose one of the following actions:
+ **Continue** – Go back to the **Configuration overview** page\. You can then continue making changes or apply pending ones\.
+ **Apply configuration** – Apply the changes you made in any of the configuration categories to your environment\. In some cases you're prompted to confirm a consequence of one of your configuration decisions\.