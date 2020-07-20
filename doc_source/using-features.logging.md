# Viewing logs from Amazon EC2 instances in your Elastic Beanstalk environment<a name="using-features.logging"></a>

The Amazon EC2 instances in your Elastic Beanstalk environment generate logs that you can view to troubleshoot issues with your application or configuration files\. Logs created by the web server, application server, Elastic Beanstalk platform scripts, and AWS CloudFormation are stored locally on individual instances\. You can easily retrieve them by using the [environment management console](environments-console.md) or the EB CLI\. You can also configure your environment to stream logs to Amazon CloudWatch Logs in real time\.

Tail logs are the last 100 lines of the most commonly used log files—Elastic Beanstalk operational logs and logs from the web server or application server\. When you request tail logs in the environment management console or with eb logs, an instance in your environment concatenates the most recent log entries into a single text file and uploads it to Amazon S3\.

Bundle logs are full logs for a wider range of log files, including logs from yum and cron and several logs from AWS CloudFormation\. When you request bundle logs, an instance in your environment packages the full log files into a ZIP archive and uploads it to Amazon S3\.

**Note**  
Elastic Beanstalk Windows Server platforms do not support bundle logs\.

To upload rotated logs to Amazon S3, the instances in your environment must have an [instance profile](concepts-roles-instance.md) with permission to write to your Elastic Beanstalk Amazon S3 bucket\. These permissions are included in the default instance profile that Elastic Beanstalk prompts you to create when you launch an environment in the Elastic Beanstalk console for the first time\.

**To retrieve instance logs**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Logs**\.

1. Choose **Request Logs**, and then choose the type of logs to retrieve\. To get tail logs, choose **Last 100 Lines**\. To get bundle logs, choose **Full Logs**\.  
![\[Environment logs page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-logs.png)

1. When Elastic Beanstalk finishes retrieving your logs, choose **Download**\.

Elastic Beanstalk stores tail and bundle logs in an Amazon S3 bucket, and generates a presigned Amazon S3 URL that you can use to access your logs\. Elastic Beanstalk deletes the files from Amazon S3 after a duration of 15 minutes\.

**Warning**  
Anyone in possession of the presigned Amazon S3 URL can access the files before they are deleted\. Make the URL available only to trusted parties\.

**Note**  
Your user policy must have the `s3:DeleteObject` permission\. Elastic Beanstalk uses your user permissions to delete the logs from Amazon S3\.

To persist logs, you can configure your environment to publish logs to Amazon S3 automatically after they are rotated\. To enable log rotation to Amazon S3, follow the procedure in [Configuring instance log viewing](environments-cfg-logging.md#environments-cfg-logging-console)\. Instances in your environment will attempt to upload logs that have been rotated once per hour\.

If your application generates logs in a location that isn't part of the default configuration for your environment's platform, you can extend the default configuration by using configuration files \(`[\.ebextensions](ebextensions.md)`\)\. You can add your application's log files to tail logs, bundle logs, or log rotation\.

For real\-time log streaming and long\-term storage, configure your environment to [stream logs to Amazon CloudWatch Logs](#health-logs-cloudwatchlogs)\.

**Topics**
+ [Log location on Amazon EC2 instances](#health-logs-instancelocation)
+ [Log location in Amazon S3](#health-logs-s3location)
+ [Log rotation settings on Linux](#health-logs-logrotate)
+ [Extending the default log task configuration](#health-logs-extend)
+ [Streaming log files to Amazon CloudWatch Logs](#health-logs-cloudwatchlogs)

## Log location on Amazon EC2 instances<a name="health-logs-instancelocation"></a>

Logs are stored in standard locations on the Amazon EC2 instances in your environment\. Elastic Beanstalk generates the following logs\.

**Linux**
+ `/var/log/eb-activity.log`
+ `/var/log/eb-commandprocessor.log`

**Windows Server**
+ `C:\Program Files\Amazon\ElasticBeanstalk\logs\`
+ `C:\cfn\logs\cfn-init.log`

These logs contain messages about deployment activities, including messages related to configuration files \([`.ebextensions`](ebextensions.md)\)\.

Each application and web server stores logs in its own folder:
+ **Apache** – `/var/log/httpd/`
+ **IIS** – `C:\inetpub\wwwroot\`
+ **Node\.js** – `/var/log/nodejs/`
+ **nginx** – `/var/log/nginx/`
+ **Passenger** – `/var/app/support/logs/`
+ **Puma** – `/var/log/puma/`
+ **Python** – `/opt/python/log/`
+ **Tomcat** – `/var/log/tomcat8/`

## Log location in Amazon S3<a name="health-logs-s3location"></a>

When you request tail or bundle logs from your environment, or when instances upload rotated logs, they're stored in your Elastic Beanstalk bucket in Amazon S3\. Elastic Beanstalk creates a bucket named `elasticbeanstalk-region-account-id` for each AWS Region in which you create environments\. Within this bucket, logs are stored under the path `resources/environments/logs/logtype/environment-id/instance-id`\. 

For example, logs from instance `i-0a1fd158`, in Elastic Beanstalk environment `e-mpcwnwheky` in AWS Region `us-west-2` in account `123456789012`, are stored in the following locations:
+ **Tail Logs** –

  `s3://elasticbeanstalk-us-west-2-123456789012/resources/environments/logs/tail/e-mpcwnwheky/i-0a1fd158`
+ **Bundle Logs** –

  `s3://elasticbeanstalk-us-west-2-123456789012/resources/environments/logs/bundle/e-mpcwnwheky/i-0a1fd158`
+ **Rotated Logs** –

  `s3://elasticbeanstalk-us-west-2-123456789012/resources/environments/logs/publish/e-mpcwnwheky/i-0a1fd158`

**Note**  
You can find your environment ID in the environment management console\.

Elastic Beanstalk deletes tail and bundle logs from Amazon S3 automatically 15 minutes after they are created\. Rotated logs persist until you delete them or move them to S3 Glacier\.

## Log rotation settings on Linux<a name="health-logs-logrotate"></a>

On Linux platforms, Elastic Beanstalk uses `logrotate` to rotate logs periodically\. If configured, after a log is rotated locally, the log rotation task picks it up and uploads it to Amazon S3\. Logs that are rotated locally don't appear in tail or bundle logs by default\.

You can find Elastic Beanstalk configuration files for `logrotate` in `/etc/logrotate.elasticbeanstalk.hourly/`\. These rotation settings are specific to the platform, and might change in future versions of the platform\. For more information about the available settings and example configurations, run `man logrotate`\.

The configuration files are invoked by cron jobs in `/etc/cron.hourly/`\. For more information about `cron`, run `man cron`\.

## Extending the default log task configuration<a name="health-logs-extend"></a>

Elastic Beanstalk uses files in subfolders of `/opt/elasticbeanstalk/tasks` \(Linux\) or `C:\Program Files\Amazon\ElasticBeanstalk\config` \(Windows Server\) on the Amazon EC2 instance to configure tasks for tail logs, bundle logs, and log rotation\.

**On Linux:**
+ **Tail Logs** –

  `/opt/elasticbeanstalk/tasks/taillogs.d/`
+ **Bundle Logs** –

  `/opt/elasticbeanstalk/tasks/bundlelogs.d/`
+ **Rotated Logs** –

  `/opt/elasticbeanstalk/tasks/publishlogs.d/`

**On Windows Server:**
+ **Tail Logs** –

  `c:\Program Files\Amazon\ElasticBeanstalk\config\taillogs.d\`
+ **Rotated Logs** –

  `c:\Program Files\Amazon\ElasticBeanstalk\config\publogs.d\`

For example, the `eb-activity.conf` file on Linux adds two log files to the tail logs task\.

**`/opt/elasticbeanstalk/tasks/taillogs.d/eb-activity.conf `**

```
/var/log/eb-commandprocessor.log
/var/log/eb-activity.log
```

You can use environment configuration files \(`[\.ebextensions](ebextensions.md)`\) to add your own `.conf` files to these folders\. A `.conf` file lists log files specific to your application, which Elastic Beanstalk adds to the log file tasks\.

Use the `files` section to add configuration files to the tasks that you want to modify\. For example, the following configuration text adds a log configuration file to each instance in your environment\. This log configuration file, `cloud-init.conf`, adds `/var/log/cloud-init.log` to tail logs\.

```
files:
  "/opt/elasticbeanstalk/tasks/taillogs.d/cloud-init.conf" :
    mode: "000755"
    owner: root
    group: root
    content: |
      /var/log/cloud-init.log
```

Add this text to a file with the `.config` file name extension to your source bundle under a folder named `.ebextensions`\.

```
~/workspace/my-app
|-- .ebextensions
|   `-- tail-logs.config
|-- index.php
`-- styles.css
```

On Linux platforms, you can also use wildcard characters in log task configurations\. This configuration file adds all files with the `.log` file name extension from the `log` folder in the application root to bundle logs\.

```
files: 
  "/opt/elasticbeanstalk/tasks/bundlelogs.d/applogs.conf" :
    mode: "000755"
    owner: root
    group: root
    content: |
      /var/app/current/log/*.log
```

**Note**  
Log task configurations don't support wildcard characters on Windows platforms\.

For more information about using configuration files, see [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)\.

Much like extending tail logs and bundle logs, you can extend log rotation using a configuration file\. Whenever Elastic Beanstalk rotates its own logs and uploads them to Amazon S3, it also rotates and uploads your additional logs\. Log rotation extension behaves differently depending on the platform's operating system\. The following sections describe the two cases\.

### Extending log rotation on Linux<a name="health-logs-extend-rotation-linux"></a>

As explained in [Log rotation settings on Linux](#health-logs-logrotate), Elastic Beanstalk uses `logrotate` to rotate logs on Linux platforms\. When you configure your application's log files for log rotation, the application doesn't need to create copies of log files\. Elastic Beanstalk configures `logrotate` to create a copy of your application's log files for each rotation\. Therefore, the application must keep log files unlocked when it isn't actively writing to them\.

### Extending log rotation on Windows server<a name="health-logs-extend-rotation-windows"></a>

On Windows Server, when you configure your application's log files for log rotation, the application must rotate the log files periodically\. Elastic Beanstalk looks for files with names starting with the pattern you configured, and picks them up for uploading to Amazon S3\. In addition, periods in the file name are ignored, and Elastic Beanstalk considers the name up to the period to be the base log file name\.

Elastic Beanstalk uploads all versions of a base log file except for the newest one, because it considers that one to be the active application log file, which can potentially be locked\. Your application can, therefore, keep the active log file locked between rotations\.

For example, your application writes to a log file named `my_log.log`, and you specify this name in your `.conf` file\. The application periodically rotates the file\. During the Elastic Beanstalk rotation cycle, it finds the following files in the log file's folder: `my_log.log`, `my_log.0800.log`, `my_log.0830.log`\. Elastic Beanstalk considers all of them to be versions of the base name `my_log`\. The file `my_log.log` has the latest modification time, so Elastic Beanstalk uploads only the other two files, `my_log.0800.log` and `my_log.0830.log`\.

## Streaming log files to Amazon CloudWatch Logs<a name="health-logs-cloudwatchlogs"></a>

You can configure your environment to stream logs to Amazon CloudWatch Logs in the Elastic Beanstalk console or by using [configuration options](command-options.md)\. With CloudWatch Logs, each instance in your environment streams logs to log groups that you can configure to be retained for weeks or years, even after your environment is terminated\.

The set of logs streamed varies per environment, but always includes `eb-activity.log` and access logs from the nginx or Apache proxy server that runs in front of your application\.

You can configure log streaming in the Elastic Beanstalk console either [during environment creation](environments-create-wizard.md#environments-create-wizard-software) or [for an existing environment](environments-cfg-logging.md#environments-cfg-logging-console)\. In the following example, logs are saved for up to seven days, even when the environment is terminated\.

![\[CloudWatch Logs settings\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/log-streaming-screen.png)

The following [configuration file](ebextensions.md) enables log streaming with 180 days retention, even if the environment is terminated\.

**Example \.ebextensions/log\-streaming\.config**  

```
option_settings:
  aws:elasticbeanstalk:cloudwatch:logs:
    StreamLogs: true
    DeleteOnTerminate: false
    RetentionInDays: 180
```