# Tagging application versions<a name="applications-versions-tagging"></a>

You can apply tags to your AWS Elastic Beanstalk application versions\. Tags are key\-value pairs associated with AWS resources\. For information about Elastic Beanstalk resource tagging, use cases, tag key and value constraints, and supported resource types, see [Tagging Elastic Beanstalk application resources](applications-tagging-resources.md)\.

You can specify tags when you create an application version\. In an existing application version, you can add or remove tags, and update the values of existing tags\. You can add up to 50 tags to each application version\.

## Adding tags during application version creation<a name="applications-versions-tagging.create"></a>

When you use the Elastic Beanstalk console to [create an environment](environments-create-wizard.md), and you choose to upload a version of your application code, you can specify tag keys and values to associate with the new application version\.

You can also use the Elastic Beanstalk console to [upload an application version](applications-versions.md) without immediately using it in an environment\. You can specify tag keys and values when you upload an application version\.

With the AWS CLI or other API\-based clients, add tags by using the `--tags` parameter on the [create\-application\-version](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-application-version.html) command\.

```
$ aws elasticbeanstalk create-application-version \
      --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 \
      --application-name my-app --version-label v1
```

When you use the EB CLI to create or update an environment, an application version is created from the code that you deploy\. There isn't a direct way to tag an application version during its creation through the EB CLI\. See the following section to learn about adding tags to an existing application version\.

## Managing tags of an existing application version<a name="applications-versions-tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk application version\.

**To manage an application version's tags using the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose your application's name from the list\.
**Note**  
If you have many applications, use the search bar to filter the application list\.

1. In the navigation pane, find your application's name and choose **Application versions**\.

1. Select the application version you want to manage\.

1. Choose **Actions**, and then choose **Manage tags**\.

1. Use the on\-screen form to add, update, or delete tags\.

1. Choose **Apply**\.

If you use the EB CLI to update your application version, use [eb tags](eb3-tags.md) to add, update, delete, or list tags\.

For example, the following command lists the tags in an application version\.

```
~/workspace/my-app$ eb tags --list --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:applicationversion/my-app/my-version"
```

The following command updates the tag `mytag1` and deletes the tag `mytag2`\.

```
~/workspace/my-app$ eb tags --update mytag1=newvalue --delete mytag2 \
      --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:applicationversion/my-app/my-version"
```

For a complete list of options and more examples, see `eb tags`\.

With the AWS CLI or other API\-based clients, use the [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html) command to list the tags of an application version\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:applicationversion/my-app/my-version"
```

Use the [update\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html) command to add, update, or delete tags in an application version\.

```
$ aws elasticbeanstalk update-tags-for-resource \
      --tags-to-add Key=mytag1,Value=newvalue --tags-to-remove mytag2 \
      --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:applicationversion/my-app/my-version"
```

Specify both tags to add and tags to update in the `--tags-to-add` parameter of update\-tags\-for\-resource\. A nonexisting tag is added, and an existing tag's value is updated\.

**Note**  
To use some of the EB CLI and AWS CLI commands with an Elastic Beanstalk application version, you need the application version's ARN\. You can retrieve the ARN by using the following command\.  

```
$ aws elasticbeanstalk describe-application-versions --application-name my-app --version-label my-version
```