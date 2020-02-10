# eb appversion<a name="eb3-appversion"></a>

## Description<a name="eb3-appversion-description"></a>

Manages your Elastic Beanstalk application versions, including deleting a version of the application or creating the application version lifecycle policy\. If you invoke the command without any options, it goes into [interactive mode](#eb3-appversion-interactive)\.

Use the `--delete` option to delete a version of the application\.

Use the `lifecycle` option to display or create the application version lifecycle policy\. Learn more at [Configuring application version lifecycle settings](applications-lifecycle.md)\.

## Syntax<a name="eb3-appversion-syntax"></a>

 eb appversion 

 eb appversion \[\-d \| \-\-delete\] *version\-label* 

 eb appversion lifecycle \[\-p \| \-\-print\] 

## Options<a name="eb3-appversion-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  \-d *version\-label* or \-\-delete *version\-label*  | Delete version version\-label of the application\. | 
|  lifecycle  | Invoke the default editor to create a new application version lifecycle policy\. Use this policy to avoid hitting the [application version quota](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html#limits_elastic_beanstalk)\. | 
|  lifecycle \-p or lifecycle \-\-print  | Display the current application lifecycle policy\. | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Using the command interactively<a name="eb3-appversion-interactive"></a>

The command without any arguments displays the versions of the application, from most recent to oldest\. See the **Examples** section for examples of what the screen looks like\. Note the status line at the bottom of the display\. It displays context\-sensitive information that you can use to guide you\.

Press `d` to delete an application version, press `l` to manage the lifecycle policy for your application, or press `q` to quit without making any changes\.

**Note**  
If the version is deployed to any environment, you cannot delete that version\.

## Output<a name="eb3-appversion-output"></a>

The command with the `--delete` *version\-label* option displays a message confirming that the application version was deleted\.

## Examples<a name="eb3-appversion-example"></a>

The following example shows the interactive window for an application with no deployments\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/InteractiveModeNoEnvironment.png)

The following example shows the interactive window for an application with the fourth version, with version label **Sample Application**, deployed\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/InteractiveModeWithEnvironment.png)

The following example shows the output from an eb appversion lifecycle \-p command, where *ACCOUNT\-ID* is the user's account ID:

```
Application details for: lifecycle
  Region: sa-east-1
  Description: Application created from the EB CLI using "eb init"
  Date Created: 2016/12/20 02:48 UTC
  Date Updated: 2016/12/20 02:48 UTC
  Application Versions: ['Sample Application']
  Resource Lifecycle Config(s):
    VersionLifecycleConfig:
      MaxCountRule:
        DeleteSourceFromS3: False
        Enabled: False
        MaxCount: 200
      MaxAgeRule:
        DeleteSourceFromS3: False
        Enabled: False
        MaxAgeInDays: 180
    ServiceRole: arn:aws:iam::ACCOUNT-ID:role/aws-elasticbeanstalk-service-role
```