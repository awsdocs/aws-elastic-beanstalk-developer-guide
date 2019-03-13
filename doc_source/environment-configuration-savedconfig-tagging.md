# Tagging Saved Configurations<a name="environment-configuration-savedconfig-tagging"></a>

You can apply tags to your AWS Elastic Beanstalk saved configurations\. Tags are key\-value pairs associated with AWS resources\. For information about Elastic Beanstalk resource tagging, use cases, tag key and value constraints, and supported resource types, see [Tagging AWS Elastic Beanstalk Application ResourcesTagging Resources](applications-tagging-resources.md)\.

You can specify tags when you create a saved configuration\. In an existing saved configuration, you can add or remove tags, and update the values of existing tags\. You can add up to 50 tags to each saved configuration\. At this time, you can manage saved configuration tags using the API or the AWS CLI\.

## Adding Tags during Saved Configuration Creation<a name="environment-configuration-savedconfig-tagging.create"></a>

With the AWS CLI or other API\-based clients, add tags by using the `--tags` parameter on the `[create\-configuration\-template](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-configuration-template.html)` command\.

```
$ aws elasticbeanstalk create-configuration-template \
      --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 \
      --application-name my-app --template-name my-template --solution-stack-name solution-stack
```

## Managing Tags of an Existing Saved Configuration<a name="environment-configuration-savedconfig-tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk saved configuration\.

With the AWS CLI or other API\-based clients, use the `[list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html)` command to list the tags of an saved configuration\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

Use the `[update\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html)` command to add, update, or delete tags in a saved configuration\.

```
$ aws elasticbeanstalk update-tags-for-resource \
      --tags-to-add Key=mytag1,Value=newvalue --tags-to-remove mytag2 \
      --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

Specify both tags to add and tags to update in the `--tags-to-add` parameter of `update-tags-for-resource`\. A nonexisting tag is added, and an existing tag's value is updated\.

**Note**  
To use these two AWS CLI commands with an Elastic Beanstalk saved configuration, you need the saved configuration's ARN\. To construct the ARN, first use the following command to retrieve the saved configuration's name\.  

```
$ aws elasticbeanstalk describe-applications --application-names my-app
```
Look for the `ConfigurationTemplates` key in the command's output\. This element shows the saved configuration's name\. Use this name where `my-template` is specified in the commands mentioned on this page\.