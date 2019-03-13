# Tagging Custom Platform Versions<a name="custom-platforms-tagging"></a>

You can apply tags to your AWS Elastic Beanstalk custom platform versions\. Tags are key\-value pairs associated with AWS resources\. For information about Elastic Beanstalk resource tagging, use cases, tag key and value constraints, and supported resource types, see [Tagging AWS Elastic Beanstalk Application ResourcesTagging Resources](applications-tagging-resources.md)\.

You can specify tags when you create a custom platform version\. In an existing custom platform version, you can add or remove tags, and update the values of existing tags\. You can add up to 50 tags to each custom platform version\. At this time, you can manage custom platform version tags using the API or the AWS CLI\.

## Adding Tags during Custom Platform Version Creation<a name="custom-platforms-tagging.create"></a>

With the AWS CLI or other API\-based clients, add tags by using the `--tags` parameter on the `[create\-platform\-version](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-platform-version.html)` command\.

```
$ aws elasticbeanstalk create-platform-version \
      --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 \
      --platform-name my-platform --platform-version 1.0.0 --platform-definition-bundle S3Bucket=my-bucket,S3Key=sample.zip
```

## Managing Tags of an Existing Custom Platform Version<a name="custom-platforms-tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk custom platform version\.

With the AWS CLI or other API\-based clients, use the `[list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html)` command to list the tags of a custom platform version\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:platform/my-platform/1.0.0"
```

Use the `[update\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html)` command to add, update, or delete tags in a custom platform version\.

```
$ aws elasticbeanstalk update-tags-for-resource \
      --tags-to-add Key=mytag1,Value=newvalue --tags-to-remove mytag2 \
      --resource-arn
      "arn:aws:elasticbeanstalk:us-east-2:my-account-id:platform/my-platform/1.0.0"
```

Specify both tags to add and tags to update in the `--tags-to-add` parameter of `update-tags-for-resource`\. A nonexisting tag is added, and an existing tag's value is updated\.

**Note**  
To use these two AWS CLI commands with an Elastic Beanstalk custom platform version, you need the custom platform version's ARN\. You can retrieve the ARN by using the following command\.  

```
$ aws elasticbeanstalk list-platform-versions
```
Use the `--filters` option to filter the output down to your custom platform's name\.