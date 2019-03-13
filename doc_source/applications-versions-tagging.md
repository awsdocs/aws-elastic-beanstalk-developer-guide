# Tagging Application Versions<a name="applications-versions-tagging"></a>

You can apply tags to your AWS Elastic Beanstalk application versions\. Tags are key\-value pairs associated with AWS resources\. For information about Elastic Beanstalk resource tagging, use cases, tag key and value constraints, and supported resource types, see [Tagging AWS Elastic Beanstalk Application ResourcesTagging Resources](applications-tagging-resources.md)\.

You can specify tags when you create an application version\. In an existing application version, you can add or remove tags, and update the values of existing tags\. You can add up to 50 tags to each application version\. At this time, you can manage application version tags using the API or the AWS CLI\.

## Adding Tags during Application Version Creation<a name="applications-versions-tagging.create"></a>

With the AWS CLI or other API\-based clients, add tags by using the `--tags` parameter on the `[create\-application\-version](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-application-version.html)` command\.

```
$ aws elasticbeanstalk create-application-version \
      --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 \
      --application-name my-app --version-label v1
```

## Managing Tags of an Existing Application Version<a name="applications-versions-tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk application version\.

With the AWS CLI or other API\-based clients, use the `[list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html)` command to list the tags of an application version\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:applicationversion/my-app/my-version"
```

Use the `[update\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html)` command to add, update, or delete tags in an application version\.

```
$ aws elasticbeanstalk update-tags-for-resource \
      --tags-to-add Key=mytag1,Value=newvalue --tags-to-remove mytag2 \
      --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:applicationversion/my-app/my-version"
```

Specify both tags to add and tags to update in the `--tags-to-add` parameter of `update-tags-for-resource`\. A nonexisting tag is added, and an existing tag's value is updated\.

**Note**  
To use these two AWS CLI commands with an Elastic Beanstalk application version, you need the application version's ARN\. You can retrieve the ARN by using the following command\.  

```
$ aws elasticbeanstalk describe-application-versions --application-name my-app --version-label my-version
```