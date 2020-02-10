# Tagging saved configurations<a name="environment-configuration-savedconfig-tagging"></a>

You can apply tags to your AWS Elastic Beanstalk saved configurations\. Tags are key\-value pairs associated with AWS resources\. For information about Elastic Beanstalk resource tagging, use cases, tag key and value constraints, and supported resource types, see [Tagging Elastic Beanstalk application resources](applications-tagging-resources.md)\.

You can specify tags when you create a saved configuration\. In an existing saved configuration, you can add or remove tags, and update the values of existing tags\. You can add up to 50 tags to each saved configuration\.

## Adding tags during saved configuration creation<a name="environment-configuration-savedconfig-tagging.create"></a>

When you use the Elastic Beanstalk console to [save a configuration](environment-configuration-savedconfig.md), you can specify tag keys and values on the **Save Configuration** page\.

![\[Save configuration page in the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-saveconfiguration-dialog.png)

If you use the EB CLI to save a configuration, use the `--tags` option with [eb config](eb3-config.md) to add tags\.

```
~/workspace/my-app$ eb config --tags mytag1=value1,mytag2=value2
```

With the AWS CLI or other API\-based clients, add tags by using the `--tags` parameter on the [create\-configuration\-template](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-configuration-template.html) command\.

```
$ aws elasticbeanstalk create-configuration-template \
      --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 \
      --application-name my-app --template-name my-template --solution-stack-name solution-stack
```

## Managing tags of an existing saved configuration<a name="environment-configuration-savedconfig-tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk saved configuration\.

**To manage a saved configuration's tags using the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Select the application whose saved configuration you want to manage\.

1. In the side navigation pane, choose **Saved configurations**\.

1. Select the saved configuration to manage, and then choose **Actions**\.

1. Choose **Manage tags**\.

   The **Manage Tags** dialog box shows the list of tags that are currently applied to the application version\.  
![\[Manage tags dialog box shows tags for a saved configuration in the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/tagging-manage-tags-dialog-savedconfiguration.png)

1. Add, update, or delete tags:
   + To add a tag, enter it into the empty boxes at the bottom of the list\.
   + To update a tag's key or value, edit the respective box in the tag's row\.
   + To delete a tag, choose ![\[Remove tag\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/x.png) next to the tag's value box\.

1. Choose **Apply**\.

If you use the EB CLI to update your saved configuration, use [eb tags](eb3-tags.md) to add, update, delete, or list tags\.

For example, the following command lists the tags in a saved configuration\.

```
~/workspace/my-app$ eb tags --list --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

The following command updates the tag `mytag1` and deletes the tag `mytag2`\.

```
~/workspace/my-app$ eb tags --update mytag1=newvalue --delete mytag2 \
      --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

For a complete list of options and more examples, see `[eb tags](eb3-tags.md)`\.

With the AWS CLI or other API\-based clients, use the [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html) command to list the tags of a saved configuration\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

Use the [update\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html) command to add, update, or delete tags in a saved configuration\.

```
$ aws elasticbeanstalk update-tags-for-resource \
      --tags-to-add Key=mytag1,Value=newvalue --tags-to-remove mytag2 \
      --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

Specify both tags to add and tags to update in the `--tags-to-add` parameter of update\-tags\-for\-resource\. A nonexisting tag is added, and an existing tag's value is updated\.

**Note**  
To use some of the EB CLI and AWS CLI commands with an Elastic Beanstalk saved configuration, you need the saved configuration's ARN\. To construct the ARN, first use the following command to retrieve the saved configuration's name\.  

```
$ aws elasticbeanstalk describe-applications --application-names my-app
```
Look for the `ConfigurationTemplates` key in the command's output\. This element shows the saved configuration's name\. Use this name where `my-template` is specified in the commands mentioned on this page\.