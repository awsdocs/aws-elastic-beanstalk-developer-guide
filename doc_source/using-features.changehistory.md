# Viewing an Elastic Beanstalk environment's change history<a name="using-features.changehistory"></a>

You can use the AWS Management Console to view a history of configuration changes that have been made to your Elastic Beanstalk environments\. Elastic Beanstalk fetches your change history from events recorded in [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) and displays them in a list that you can easily navigate and filter\.

The Change History panel displays the following information for changes made to your environments:
+ The date and time when a change was made
+ The IAM user that was responsible for a change made
+ The source tool \(either Elastic Beanstalk command line interface \(EB CLI\) or console\) that was used to make the change
+ The configuration parameter and new values that were set

Any sensitive data that is part of the change, such as the names of database users affected by the change, aren't displayed in the panel\.

**To view change history**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Change history**\.  
![\[Change History page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-change-history.png)

   The Change History page shows a list of configuration changes that were made to your Elastic Beanstalk environments\. You can page through the list by choosing **<** \(previous\) or **>** \(next\), or by choosing a specific page number\. Under the **Configuration changes** column, select the arrow icon to toggle between expanding and collapsing the list of changes under the **Changes made** heading\. Use the search bar to filter your results from the change history list\. You can enter any string to narrow down the list of changes that are displayed\. 

 Note the following about filtering the displayed results: 
+  The search filter is not case sensitive\. 
+  You can filter displayed changes based on information under the **Configuration changes** column, even when it is not visible due to being collapsed inside **Changes made**\. 
+  You can only filter the results displayed\. However, the filter remains in place even if you select to go to another page to display more results\. Your filtered results also append to the result set of the next page\. 

 The following examples demonstrate how the data shown on the earlier screen can be filtered:
+  Enter **GettingStartedApp\-env** in the search box to narrow down the results to only include the changes that were made to the environment named *GettingStartedApp\-env*\. 
+  Enter **example3** in the search box to narrow down the results to only include changes that were made by IAM users whose username contains the string *example3*\. 
+  Enter **2020\-10** in the search box to narrow down the results to only include changes that were made during the month of October 2020\. Change the search value to **2020\-10\-16** to filter further the displayed results to only include changes that were made on the day of October 16, 2020\. 
+  Enter **proxy:staticfiles** in the search box to narrow down the results to only include the changes that were made to the namespace named *aws:elasticbeanstalk:environment:proxy:staticfiles*\. The rows that are displayed are the result of the filter\. This is true even for results that are collapsed under **Changes made**\.