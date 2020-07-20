# Tagging custom platform versions<a name="custom-platforms-tagging"></a>

You can apply tags to your AWS Elastic Beanstalk custom platform versions\. Tags are key\-value pairs associated with AWS resources\. For information about Elastic Beanstalk resource tagging, use cases, tag key and value constraints, and supported resource types, see [Tagging Elastic Beanstalk application resources](applications-tagging-resources.md)\.

You can specify tags when you create a custom platform version\. In an existing custom platform version, you can add or remove tags, and update the values of existing tags\. You can add up to 50 tags to each custom platform version\.

## Adding tags during custom platform version creation<a name="custom-platforms-tagging.create"></a>

If you use the EB CLI to create your custom platform version, use the `--tags` option with [eb platform create](eb3-platform.md#eb3-platform-create) to add tags\.

```
~/workspace/my-app$ eb platform create --tags mytag1=value1,mytag2=value2
```

With the AWS CLI or other API\-based clients, add tags by using the `--tags` parameter on the [create\-platform\-version](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-platform-version.html) command\.

```
$ aws elasticbeanstalk create-platform-version \
      --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 \
      --platform-name my-platform --platform-version 1.0.0 --platform-definition-bundle S3Bucket=my-bucket,S3Key=sample.zip
```

## Managing tags of an existing custom platform version<a name="custom-platforms-tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk custom platform version\.

If you use the EB CLI to update your custom platform version, use [eb tags](eb3-tags.md) to add, update, delete, or list tags\.

For example, the following command lists the tags in a custom platform version\.

```
~/workspace/my-app$ eb tags --list --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:platform/my-platform/1.0.0"
```

The following command updates the tag `mytag1` and deletes the tag `mytag2`\.

```
~/workspace/my-app$ eb tags --update mytag1=newvalue --delete mytag2 \
      --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:platform/my-platform/1.0.0"
```

For a complete list of options and more examples, see `eb tags`\.

With the AWS CLI or other API\-based clients, use the [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html) command to list the tags of a custom platform version\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:platform/my-platform/1.0.0"
```

Use the [update\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html) command to add, update, or delete tags in a custom platform version\.

```
$ aws elasticbeanstalk update-tags-for-resource \
      --tags-to-add Key=mytag1,Value=newvalue --tags-to-remove mytag2 \
      --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:platform/my-platform/1.0.0"
```

Specify both tags to add and tags to update in the `--tags-to-add` parameter of update\-tags\-for\-resource\. A nonexisting tag is added, and an existing tag's value is updated\.

**Note**  
To use some of the EB CLI and AWS CLI commands with an Elastic Beanstalk custom platform version, you need the custom platform version's ARN\. You can retrieve the ARN by using the following command\.  

```
$ aws elasticbeanstalk list-platform-versions
```
Use the `--filters` option to filter the output down to your custom platform's name\.