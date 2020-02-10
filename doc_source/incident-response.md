# Logging and monitoring in Elastic Beanstalk<a name="incident-response"></a>

Monitoring is important for maintaining the reliability, availability, and performance of AWS Elastic Beanstalk and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multipoint failure if one occurs\. AWS provides several tools for monitoring your Elastic Beanstalk resources and responding to potential incidents\.

For more information about monitoring, see [Monitoring an environment](environments-health.md)\.

For other Elastic Beanstalk security topics, see [AWS Elastic Beanstalk security](security.md)\.

## Enhanced health reporting<a name="incident-response.healthd"></a>

Enhanced health reporting is a feature that you can enable on your environment to allow Elastic Beanstalk to gather additional information about resources in your environment\. Elastic Beanstalk analyzes the information to provide a better picture of overall environment health and help identify issues that can cause your application to become unavailable\. For more information, see [Enhanced health reporting and monitoring](health-enhanced.md)\.

## Amazon EC2 instance logs<a name="incident-response.instance-logs"></a>

The Amazon EC2 instances in your Elastic Beanstalk environment generate logs that you can view to troubleshoot issues with your application or configuration files\. Logs created by the web server, application server, Elastic Beanstalk platform scripts, and AWS CloudFormation are stored locally on individual instances\. You can easily retrieve them by using the [environment management console](environments-console.md) or the EB CLI\. You can also configure your environment to stream logs to Amazon CloudWatch Logs in real time\. For more information, see [Viewing logs from Amazon EC2 instances in your Elastic Beanstalk environment](using-features.logging.md)\. 

## Environment notifications<a name="incident-response.env-notifications"></a>

You can configure your Elastic Beanstalk environment to use Amazon Simple Notification Service \(Amazon SNS\) to notify you of important events that affect your application\. Specify an email address during or after environment creation to receive emails from AWS when an error occurs, or when your environment's health changes\. For more information, see [Elastic Beanstalk environment notifications with Amazon SNS](using-features.managing.sns.md)\.

## Amazon CloudWatch alarms<a name="incident-response.alarms"></a>

Using CloudWatch alarms, you watch a single metric over a time period that you specify\. If the metric exceeds a given threshold, a notification is sent to an Amazon SNS topic or AWS Auto Scaling policy\. CloudWatch alarms don't invoke actions because they are in a particular state\. Instead, alarms invoke actions when the state changed and was maintained for a specified number of periods\. For more information, see [Using Elastic Beanstalk with Amazon CloudWatch](AWSHowTo.cloudwatch.md)\.

## AWS CloudTrail logs<a name="incident-response.cloudtrail-logs"></a>

CloudTrail provides a record of actions taken by a user, role, or an AWS service in Elastic Beanstalk\. Using the information collected by CloudTrail, you can determine the request that was made to Elastic Beanstalk, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, see [Logging Elastic Beanstalk API calls with AWS CloudTrail](AWSHowTo.cloudtrail.md)\.

## AWS X\-Ray debugging<a name="incident-response.xray"></a>

X\-Ray is an AWS service that gathers data about the requests that your application serves, and uses it to construct a service map that you can use to identify issues with your application and opportunities for optimization\. You can use the AWS Elastic Beanstalk console or a configuration file to run the X\-Ray daemon on the instances in your environment\. For more information, see [Configuring AWS X\-Ray debugging](environment-configuration-debugging.md)\.