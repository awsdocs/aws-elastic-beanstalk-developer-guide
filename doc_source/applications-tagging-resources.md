# Tagging Elastic Beanstalk application resources<a name="applications-tagging-resources"></a>

You can apply tags to resources of your AWS Elastic Beanstalk applications\. Tags are key\-value pairs associated with AWS resources\. Tags can help you categorize resources\. They're particularly useful if you manage many resources as part of multiple AWS applications\.

Here are some ways to use tagging with Elastic Beanstalk resources:
+ *Deployment stages* – Identify resources associated with different stages of your application, such as development, beta, and production\.
+ *Cost allocation* – Use cost allocation reports to track your usage of AWS resources associated with various expense accounts\. The reports include both tagged and untagged resources, and they aggregate costs according to tags\. For information about how cost allocation reports use tags, see [Use Cost Allocation Tags for Custom Billing Reports](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/allocation.html) in the *AWS Billing and Cost Management User Guide*\.
+ *Access control* – Use tags to manage permissions to requests and resources\. For example, a user who can only create and manage beta environments should only have access to beta stage resources\. For details, see [Using tags to control access to Elastic Beanstalk resources](AWSHowTo.iam.policies.access-tags.md)\.

You can add up to 50 tags to each resource\. Environments are slightly different: Elastic Beanstalk adds three default system tags to environments, and you can't edit or delete these tags\. In addition to the default tags, you can add up to 47 additional tags to each environment\.

The following constraints apply to tag keys and values:
+ Keys and values can contain letters, numbers, white space, and the following symbols: `_ . : / = + - @`
+ Keys can contain up to 127 characters\. Values can contain up to 255 characters\.
**Note**  
These length limits are for Unicode characters in UTF\-8\. For other multibyte encodings, the limits might be lower\.
+ Keys are case sensitive\.
+ Keys cannot begin with `aws:` or `elasticbeanstalk:`\.

## Resources you can tag<a name="applications-tagging-resources.supported"></a>

The following are the types of Elastic Beanstalk resources that you can tag, and links to specific topics about managing tags for each of them:
+ [Applications](applications-tagging.md)
+ [Environments](using-features.tagging.md)
+ [Application versions](applications-versions-tagging.md)
+ [Saved configurations](environment-configuration-savedconfig-tagging.md)
+ [Custom platform versions](custom-platforms-tagging.md)