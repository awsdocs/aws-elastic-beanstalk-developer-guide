# Tagging Resources in Your Elastic Beanstalk Environment<a name="using-features.tagging"></a>

## Introduction to Environment Tagging<a name="using-features.tagging.intro"></a>

AWS Elastic Beanstalk provides support for you to specify tags to apply to resources in your environment\. Tags are key\-value pairs\. They help you identify environments in cost allocation reports\. This is especially useful if you manage many environments\. You can also use tags to manage permissions at the resource level\. For more information, see [Tagging Your Amazon EC2 Resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances*\.

By default, Elastic Beanstalk applies three tags:

+ `elasticbeanstalk:environment-name` – The name of the environment\. 

+ `elasticbeanstalk:environment-id` – The environment ID\.

+ `Name` – Also the name of the environment\. `Name` is used in the Amazon EC2 dashboard to identify and sort resources\.

You can't edit these default tags\.

You can specify tags when you create the Elastic Beanstalk environment\. In an existing environment, you can add or remove tags, and you can update the values of existing tags\. In addition to the default tags, you can add up to 47 additional tags to each environment\.

**Constraints**

+ Keys and values can contain letters, numbers, white space, and the following symbols: `_ . : / = + - @`

+ Keys can contain up to 128 characters\. Values can contain up to 256 characters\.

+ Keys are case\-sensitive\.

+ Keys cannot begin with `aws:` or `elasticbeanstalk:`\.

+ Values cannot match the environment name\.

You can use cost allocation reports to track your usage of AWS resources\. The reports include both tagged and untagged resources, but they aggregate costs according to tags\. For information about how cost allocation reports use tags, see [Use Cost Allocation Tags for Custom Billing Reports](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/allocation.html) in the *AWS Billing and Cost Management User Guide*\.

## Adding Tags During Environment Creation<a name="using-features.tagging.create"></a>

When you use the Elastic Beanstalk console to create an environment, you can specify tag keys and values on the **Tags** page of the Create New Environment wizard, as shown\.

![\[Tags page where you specify tag keys and values\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-create-tags.png)

If you use the EB CLI to create environments, use the `--tags` option with `eb create` to add tags\.

```
~/workspace/my-app$ eb create --tags mytag1=value1,mytag2=value2
```

With the AWS CLI or other API\-based clients, use the `--tags` parameter on the `[create\-environment](http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-environment.html)` command\.

```
$ aws elasticbeanstalk create-environment --tags Key=mytag1,Value=value1 Key=mytag2,Value=value2 --application-name my-app --environment-name my-env --cname-prefix my-app --version-label v1 --template-name my-saved-config
```

Saved configurations include user\-defined tags\. When you apply a saved configuration that contains tags during environment creation, those tags are applied to the new environment, as long as you don't specify any new tags\. If you add tags to an environment using one of the preceding methods, any tags defined in the saved configuration are discarded\.

## Managing Tags of an Existing Environment<a name="using-features.tagging.manage"></a>

You can add, update, and delete tags in an existing Elastic Beanstalk environment\. Elastic Beanstalk applies the changes to your environment's resources\.

However, you can't edit the default tags that Elastic Beanstalk applies to your environment\. For details about these default tags, see \.

**To manage an environment's tags in the Elastic Beanstalk console**

1. Navigate to the environment management console for your environment\.

1. In the side navigation pane, choose **Tags**\.

   The tag management page shows the list of tags that currently exist in the environment\.  
![\[Tag management page shows tags for the environment\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-manage-tags.png)

1. Add, update, or delete tags:

   + To add a tag, type it into the empty boxes at the bottom of the list\.

   + To update a tag's key or value, edit the respective box in the tag's row\.

   + To delete a tag, choose ![\[Remove tag\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/x.png) next to the tag's value box\.

1. Choose **Apply**\.

If you use the EB CLI to update environments, use `eb tags` to add, update, delete, or list tags\.

For example, the following command lists the tags in your default environment\.

```
~/workspace/my-app$ eb tags --list
```

The following command updates the tag `mytag1` and deletes the tag `mytag2`\.

```
~/workspace/my-app$ eb tags --update mytag1=newvalue --delete mytag2
```

For a complete list of options and more examples, see `eb tags`\.

With the AWS CLI or other API\-based clients, use the `[list\-tags\-for\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/list-tags-for-resource.html)` command to list the tags of an environment\.

```
$ aws elasticbeanstalk list-tags-for-resource --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:environment/my-app/my-env"
```

Use the `[update\-tags\-for\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-tags-for-resource.html)` command to add, update, or delete tags in an environment\.

```
$ aws elasticbeanstalk update-tags-for-resource --tags-to-add
      Key=mytag1,Value=newvalue --tags-to-remove mytag2 --resource-arn "arn:aws:elasticbeanstalk:us-east-2:my-account-id:environment/my-app/my-env"
```

**Note**  
To use these two commands with an Elastic Beanstalk environment, you need the environment's ARN\. You can retrieve the ARN by using the following command\.  

```
aws elasticbeanstalk describe-environments
```
Specify both tags to add and tags to update in the `--tags-to-add` parameter of `update-tags-for-resource`\. A nonexisting tag is added, and an existing tag's value is updated\.