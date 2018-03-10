# Viewing an Elastic Beanstalk Environment's Event Stream<a name="using-features.events"></a>

You can use the AWS Management Console to access events and notifications associated with your application\.

**To view events**

1. Navigate to the [management page](environments-console.md) for your environment\.

1. From the navigation menu, click **Events**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-events.png)

   The Events page shows you a list of all events that have been recorded for the environment and application version\. You can filter on the type of events by using the **Severity** drop\-down list\. You can also filter when the events occurred by using the time slider\. 

The [EB CLI](eb-cli3.md) and [AWS CLI](https://aws.amazon.com/cli/) both provide commands for retrieving events\. If you are managing your environment using the EB CLI, use [`eb events`](eb3-events.md) to print a list of events\. This command also has a `--follow` option that continues to show new events until you press **Ctrl\-C** to stop output\.

To pull events using the AWS CLI, use the `describe-events` command and specify the environment by name or ID:

```
$ aws elasticbeanstalk describe-events --environment-id e-gbjzqccra3
{
    "Events": [
        {
            "ApplicationName": "elastic-beanstalk-example",
            "EnvironmentName": "elasticBeanstalkExa-env",
            "Severity": "INFO",
            "RequestId": "a4c7bfd6-2043-11e5-91e2-9114455c358a",
            "Message": "Environment update completed successfully.",
            "EventDate": "2015-07-01T22:52:12.639Z"
        },
...
```

For more information on the command line tools, see [Tools](eb-cli3.md)\.