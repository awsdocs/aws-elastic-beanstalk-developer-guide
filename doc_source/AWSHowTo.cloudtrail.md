# Logging Elastic Beanstalk API Calls with AWS CloudTrail<a name="AWSHowTo.cloudtrail"></a>

Elastic Beanstalk is integrated with CloudTrail, a service that captures all of the Elastic BeanstalkAPI calls and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the Elastic Beanstalk console or from your code to the Elastic Beanstalk APIs\. Using the information collected by CloudTrail, you can determine the request that was made to Elastic Beanstalk, the source IP address from which the request was made, who made the request, when it was made, and so on\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Elastic Beanstalk Information in CloudTrail History<a name="elastic-beanstalk-info-in-cloudtrail-history"></a>

The CloudTrail API activity history feature lets you look up and filter events captured by CloudTrail\. You can look up events related to the creation, modification, or deletion of resources in your AWS account on a per\-region basis\. Events can be looked up by using the CloudTrail console, or programmatically by using the AWS SDKs or AWS CLI \([lookup\-events](http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/lookup-events.html)\)\. 

For a list of supported actions, see [AWS Elastic Beanstalk APIs](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events-supported-services.html#view-cloudtrail-events-supported-apis-elasticbeanstalk) in the *CloudTrail User Guide*\.

## Elastic Beanstalk Information in CloudTrail Logging<a name="elastic-beanstalk-info-in-cloudtrail-logging"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to Elastic Beanstalk actions are tracked in CloudTrail log files, where they are written with other AWS service records\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All Elastic Beanstalk actions are logged by CloudTrail and are documented in the [Elastic Beanstalk API Reference](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/)\. For example, calls to the **CreateEnvironment**, **UpdateEnvironment** and **TerminateEnvironment** sections generate entries in the CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log entry helps you determine the following: 

+ Whether the request was made with root or IAM user credentials

+ Whether the request was made with temporary security credentials for a role or federated user

+ Whether the request was made by another AWS service

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

You can store your log files in your Amazon S3 bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

If you want to be notified upon log file delivery, you can configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Elastic Beanstalk log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. 

For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

## Understanding Elastic Beanstalk Log File Entries<a name="understanding-elastic-beanstalk-entries"></a>

CloudTrail log files can contain one or more log entries\. Each entry lists multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. Log entries are not an ordered stack trace of the public API calls, so they do not appear in any specific order\.

The following example shows a CloudTrail log entry that demonstrates the `UpdateEnvironment` action called by an IAM user named `intern`\.

```
{
  "Records": [{
    "eventVersion": "1.03",
    "userIdentity": {
      "type": "IAMUser",
      "principalId": "AIXDAYQEXAMPLEUMLYNGL",
      "arn": "arn:aws:iam::123456789012:user/intern",
      "accountId": "123456789012",
      "accessKeyId": "ASXIAGXEXAMPLEQULKNXV",
      "userName": "intern",
      "sessionContext": {
        "attributes": {
          "mfaAuthenticated": "false",
          "creationDate": "2016-04-22T00:23:24Z"
        }
      },
      "invokedBy": "signin.amazonaws.com"
    },
    "eventTime": "2016-04-22T00:24:14Z",
    "eventSource": "elasticbeanstalk.amazonaws.com",
    "eventName": "UpdateEnvironment",
    "awsRegion": "us-west-2",
    "sourceIPAddress": "255.255.255.54",
    "userAgent": "signin.amazonaws.com",
    "requestParameters": {
      "optionSettings": []
    },
    "responseElements": null,
    "requestID": "84ae9ecf-0280-17ce-8612-705c7b132321",
    "eventID": "e48b6a08-c6be-4a22-99e1-c53139cbfb18",
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
  }]
}
```