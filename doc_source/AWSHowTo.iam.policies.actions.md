# Resources and conditions for Elastic Beanstalk actions<a name="AWSHowTo.iam.policies.actions"></a>

This section describes the resources and conditions that you can use in policy statements to grant permissions that allow specific Elastic Beanstalk actions to be performed on specific Elastic Beanstalk resources\.

Conditions enable you to specify permissions to resources that the action needs to complete\. For example, when you can call the `CreateEnvironment` action, you must also specify the application version to deploy as well as the application that contains that application name\. When you set permissions for the `CreateEnvironment` action, you specify the application and application version that you want the action to act upon by using the `InApplication` and `FromApplicationVersion` conditions\. 

In addition, you can specify the environment configuration with a solution stack \(`FromSolutionStack`\) or a configuration template \(`FromConfigurationTemplate`\)\. The following policy statement allows the `CreateEnvironment` action to create an environment with the name **myenv** \(specified by `Resource`\) in the application **My App** \(specified by the `InApplication` condition\) using the application version **My Version** \(`FromApplicationVersion`\) with a **32bit Amazon Linux running Tomcat 7** configuration \(`FromSolutionStack`\):

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:CreateEnvironment"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"],
          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-east-2::solutionstack/32bit Amazon Linux running Tomcat 7"]
        }
      }
    }
  ]
}
```

**Note**  
Most condition keys mentioned in this topic are specific to Elastic Beanstalk, and their names contain the `elasticbeanstalk:` prefix\. For brevity, we omit this prefix from the condition key names when we mention them in the following sections\. For example, we mention `InApplication` instead of its full name `elasticbeanstalk:InApplication`\.  
In contrast, we mention a few condition keys used across AWS services, and we include their `aws:` prefix to highlight the exception\.  
Policy examples always show full condition key names, including the prefix\.

**Topics**
+ [Policy information for Elastic Beanstalk actions](#AWSHowTo.iam.policies.actions.table)
+ [Condition keys for Elastic Beanstalk actions](#AWSHowTo.iam.policies.conditions)

## Policy information for Elastic Beanstalk actions<a name="AWSHowTo.iam.policies.actions.table"></a>

The following table lists all Elastic Beanstalk actions, the resource that each action acts upon, and the additional contextual information that can be provided using conditions\.


**Policy information for Elastic Beanstalk actions, including resources, conditions, examples, and dependencies**  

| Resource | Conditions | Example statement | 
| --- | --- | --- | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_AbortEnvironmentUpdate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_AbortEnvironmentUpdate.html) | 
|  `application` `environment`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows a user to abort environment update operations on environments in an application named `My App`\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:AbortEnvironmentUpdate"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CheckDNSAvailability.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CheckDNSAvailability.html) | 
|  `"*"`  |  N/A  |  <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CheckDNSAvailability"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": "*"<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ComposeEnvironments.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ComposeEnvironments.html) | 
|  `application`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows a user to compose environments that belong to an application named `My App`\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:ComposeEnvironments"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplication.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplication.html) | 
|  `application`  |  `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  This example allows the `CreateApplication` action to create applications whose names begin with **DivA**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CreateApplication"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/DivA*"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplicationVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplicationVersion.html) | 
|  `applicationversion`  |  `InApplication` `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  This example allows the `CreateApplicationVersion` action to create application versions with any name \(**\***\) in the application **My App**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CreateApplicationVersion"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/*"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateConfigurationTemplate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateConfigurationTemplate.html) | 
|  `configurationtemplate`  |  `InApplication` `FromApplication` `FromApplicationVersion` `FromConfigurationTemplate` `FromEnvironment` `FromSolutionStack` `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `CreateConfigurationTemplate` action to create configuration templates whose name begins with **My Template** \(`My Template*`\) in the application **My App**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CreateConfigurationTemplate"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:configurationtemplate/My App/My Template*"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],<br />          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-east-2::solutionstack/32bit Amazon Linux running Tomcat 7"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateEnvironment.html) | 
|  `environment`  |  `InApplication` `FromApplicationVersion` `FromConfigurationTemplate` `FromSolutionStack` `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `CreateEnvironment` action to create an environment whose name is **myenv** in the application **My App** and using the solution stack **32bit Amazon Linux running Tomcat 7**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CreateEnvironment"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],<br />          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"],<br />          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-east-2::solutionstack/32bit Amazon Linux running Tomcat 7"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreatePlatformVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreatePlatformVersion.html) | 
|  `platform`  |  `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  This example allows the `CreatePlatformVersion` action to create platform versions targeting the `us-east-2` region, whose names begin with **us\-east\-2\_**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CreatePlatformVersion"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:platform/us-east-2_*"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateStorageLocation.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateStorageLocation.html) | 
|  `"*"`  |  N/A  |  <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:CreateStorageLocation"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": "*"<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplication.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplication.html) | 
|  `application`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DeleteApplication` action to delete the application **My App**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DeleteApplication"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplicationVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplicationVersion.html) | 
|  `applicationversion`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DeleteApplicationVersion` action to delete an application version whose name is **My Version** in the application **My App**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DeleteApplicationVersion"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }        <br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteConfigurationTemplate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteConfigurationTemplate.html) | 
|  `configurationtemplate`  |  `InApplication` \(Optional\) `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DeleteConfigurationTemplate` action to delete a configuration template whose name is **My Template** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DeleteConfigurationTemplate"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:configurationtemplate/My App/My Template"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteEnvironmentConfiguration.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteEnvironmentConfiguration.html) | 
|  `environment`  |  `InApplication` \(Optional\)  |  The following policy allows the `DeleteEnvironmentConfiguration` action to delete a draft configuration for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DeleteEnvironmentConfiguration"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeletePlatformVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeletePlatformVersion.html) | 
|  `platform`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DeletePlatformVersion` action to delete platform versions targeting the `us-east-2` region, whose names begin with **us\-east\-2\_**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DeletePlatformVersion"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:platform/us-east-2_*"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplications.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplications.html) | 
|  `application`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeApplications` action to describe the application My App\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DescribeApplications"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplicationVersions.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplicationVersions.html) | 
|  `applicationversion`  |  `InApplication` \(Optional\) `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeApplicationVersions` action to describe the application version **My Version** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DescribeApplicationVersions"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationOptions.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationOptions.html) | 
|  `environment` `configurationtemplate` `solutionstack`  |  `InApplication` \(Optional\) `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeConfigurationOptions` action to describe the configuration options for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeConfigurationOptions",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationSettings.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationSettings.html) | 
|  `environment` `configurationtemplate`  |  `InApplication` \(Optional\) `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeConfigurationSettings` action to describe the configuration settings for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeConfigurationSettings",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentHealth.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentHealth.html) | 
|  `environment`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows use of `DescribeEnvironmentHealth` to retrieve health information for an environment named **myenv**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeEnvironmentHealth",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentResources.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentResources.html) | 
|  `environment`  |  `InApplication` \(Optional\) `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeEnvironmentResources` action to return list of AWS resources for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeEnvironmentResources",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironments.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironments.html) | 
|  `environment`  |  `InApplication` \(Optional\) `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeEnvironments` action to describe the environments **myenv** and **myotherenv** in the application **My App**\. Specifying the application name as a condition is optional\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeEnvironments",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv",<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App2/myotherenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEvents.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEvents.html) | 
|  `application` `applicationversion` `configurationtemplate` `environment`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribeEvents` action to list event descriptions for the environment **myenv** and the application version **My Version** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeEvents",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv",<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeInstancesHealth.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeInstancesHealth.html) | 
|  `environment`  |  N/A  |  The following policy allows use of `DescribeInstancesHealth` to retrieve health information for instances in an environment named **myenv**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": "elasticbeanstalk:DescribeInstancesHealth",<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribePlatformVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribePlatformVersion.html) | 
|  `platform`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `DescribePlatformVersion` action to describe platform versions targeting the `us-east-2` region, whose names begin with **us\-east\-2\_**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:DescribePlatformVersion"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:platform/us-east-2_*"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html) | 
|  `solutionstack`  |  N/A  |  The following policy allows the `ListAvailableSolutionStacks` action to return only the solution stack **32bit Amazon Linux running Tomcat 7**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:ListAvailableSolutionStacks"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": "arn:aws:elasticbeanstalk:us-east-2::solutionstack/32bit Amazon Linux running Tomcat 7"<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListPlatformVersions.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListPlatformVersions.html) | 
|  `platform`  |  `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  This example allows the `CreatePlatformVersion` action to create platform versions targeting the `us-east-2` region, whose names begin with **us\-east\-2\_**: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:ListPlatformVersions"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:platform/us-east-2_*"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListTagsForResource.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListTagsForResource.html) | 
|  `application` `applicationversion` `configurationtemplate` `environment` `platform`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `ListTagsForResource` action to list tags of existing resources only if they have a tag named `stage` with the value `test`: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:ListTagsForResource"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": "*",<br />      "Condition": {<br />        "StringEquals": {<br />          "aws:ResourceTag/stage": ["test"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RebuildEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RebuildEnvironment.html) | 
|  `environment`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `RebuildEnvironment` action to rebuild the environment **myenv** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:RebuildEnvironment"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RequestEnvironmentInfo.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RequestEnvironmentInfo.html) | 
|  `environment`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `RequestEnvironmentInfo` action to compile information about the environment **myenv** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:RequestEnvironmentInfo"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RestartAppServer.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RestartAppServer.html) | 
|  `environment`  |  `InApplication`  |  The following policy allows the `RestartAppServer` action to restart the application container server for the environment **myenv** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:RestartAppServer"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RetrieveEnvironmentInfo.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RetrieveEnvironmentInfo.html) | 
|  `environment`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `RetrieveEnvironmentInfo` action to retrieve the compiled information for the environment **myenv** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:RetrieveEnvironmentInfo"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_SwapEnvironmentCNAMEs.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_SwapEnvironmentCNAMEs.html) | 
|  `environment`  |  `InApplication` \(Optional\) `FromEnvironment` \(Optional\)  |  The following policy allows the `SwapEnvironmentCNAMEs` action to swap the CNAMEs for the environments **mysrcenv** and **mydestenv**\.  <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:SwapEnvironmentCNAMEs"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/mysrcenv",<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/mydestenv"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html) | 
|  `environment`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `TerminateEnvironment` action to terminate the environment **myenv** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:TerminateEnvironment"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[UpdateApplication](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplication.html) | 
|  `application`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `UpdateApplication` action to update properties of the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:UpdateApplication"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[UpdateApplicationResourceLifecycle](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplicationResourceLifecycle.html) | 
|  `application`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `UpdateApplicationResourceLifecycle` action to update lifecycle settings of the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:UpdateApplicationResourceLifecycle"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"<br />      ]<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplicationVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplicationVersion.html) | 
|  `applicationversion`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `UpdateApplicationVersion` action to update the properties of the application version **My Version** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:UpdateApplicationVersion"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateConfigurationTemplate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateConfigurationTemplate.html) | 
|  `configurationtemplate`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `UpdateConfigurationTemplate` action to update the properties or options of the configuration template **My Template** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:UpdateConfigurationTemplate"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:configurationtemplate/My App/My Template"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateEnvironment.html) | 
|  `environment`  |  `InApplication` `FromApplicationVersion` `FromConfigurationTemplate` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `UpdateEnvironment` action to update the environment **myenv** in the application **My App** by deploying the application version **My Version**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:UpdateEnvironment"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],<br />          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[ `UpdateTagsForResource`](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html) – `AddTags` | 
|  `application` `applicationversion` `configurationtemplate` `environment` `platform`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:RequestTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The `AddTags` action is one of two virtual actions associated with the [http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html) API\. The following policy allows the `AddTags` action to modify tags of existing resources only if they have a tag named `stage` with the value `test`: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:AddTags"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": "*",<br />      "Condition": {<br />        "StringEquals": {<br />          "aws:ResourceTag/stage": ["test"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[ `UpdateTagsForResource`](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html) – `RemoveTags` | 
|  `application` `applicationversion` `configurationtemplate` `environment` `platform`  |  `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The `RemoveTags` action is one of two virtual actions associated with the [http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html) API\. The following policy denies the `RemoveTags` action to request the removal of a tag named `stage` from existing resources: <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:RemoveTags"<br />      ],<br />      "Effect": "Deny",<br />      "Resource": "*",<br />      "Condition": {<br />        "ForAnyValue:StringEquals": {<br />          "aws:TagKeys": ["stage"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ValidateConfigurationSettings.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ValidateConfigurationSettings.html) | 
|  `template` `environment`  |  `InApplication` `aws:ResourceTag/key-name` \(Optional\) `aws:TagKeys` \(Optional\)  |  The following policy allows the `ValidateConfigurationSettings` action to validates configuration settings against the environment **myenv** in the application **My App**\. <pre>{<br />  "Version": "2012-10-17",<br />  "Statement": [<br />    {<br />      "Action": [<br />        "elasticbeanstalk:ValidateConfigurationSettings"<br />      ],<br />      "Effect": "Allow",<br />      "Resource": [<br />        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"<br />      ],<br />      "Condition": {<br />        "StringEquals": {<br />          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]<br />        }<br />      }<br />    }<br />  ]<br />}</pre>  | 

## Condition keys for Elastic Beanstalk actions<a name="AWSHowTo.iam.policies.conditions"></a>

Keys enable you to specify conditions that express dependencies, restrict permissions, or specify constraints on the input parameters for an action\. Elastic Beanstalk supports the following keys\.

`InApplication`  
Specifies the application that contains the resource that the action operates on\.  
The following example allows the `UpdateApplicationVersion` action to update the properties of the application version **My Version**\. The `InApplication` condition specifies **My App** as the container for **My Version**\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:UpdateApplicationVersion"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```

`FromApplicationVersion`  
Specifies an application version as a dependency or a constraint on an input parameter\.  
The following example allows the `UpdateEnvironment` action to update the environment **myenv** in the application **My App**\. The `FromApplicationVersion` condition constrains the `VersionLabel` parameter to allow only the application version **My Version** to update the environment\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:UpdateEnvironment"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My App/My Version"]
        }
      }
    }
  ]
}
```

`FromConfigurationTemplate`  
Specifies a configuration template as a dependency or a constraint on an input parameter\.  
The following example allows the `UpdateEnvironment` action to update the environment **myenv** in the application **My App**\. The `FromConfigurationTemplate` condition constrains the `TemplateName` parameter to allow only the configuration template **My Template** to update the environment\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:UpdateEnvironment"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],
          "elasticbeanstalk:FromConfigurationTemplate": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:configurationtemplate/My App/My Template"]
        }
      }
    }
  ]
}
```

`FromEnvironment`  
Specifies an environment as a dependency or a constraint on an input parameter\.  
The following example allows the `SwapEnvironmentCNAMEs` action to swap the CNAMEs in **My App** for all environments whose names begin with **mysrcenv** and **mydestenv** but not those environments whose names begin with **mysrcenvPROD\*** and **mydestenvPROD\***\.   

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:SwapEnvironmentCNAMEs"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/mysrcenv*",
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/mydestenv*"
      ],
      "Condition": {
        "StringNotLike": {
          "elasticbeanstalk:FromEnvironment": [
            "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/mysrcenvPROD*",
            "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My App/mydestenvPROD*"
          ]
        }
      }
    }
  ]
}
```

`FromSolutionStack`  
Specifies a solution stack as a dependency or a constraint on an input parameter\.  
The following policy allows the `CreateConfigurationTemplate` action to create configuration templates whose name begins with **My Template** \(`My Template*`\) in the application **My App**\. The `FromSolutionStack` condition constrains the `solutionstack` parameter to allow only the solution stack **32bit Amazon Linux running Tomcat 7** as the input value for that parameter\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:CreateConfigurationTemplate"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:configurationtemplate/My App/My Template*"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My App"],
          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-east-2::solutionstack/32bit Amazon Linux running Tomcat 7"]
        }
      }
    }
  ]
}
```

`aws:ResourceTag/key-name``aws:RequestTag/key-name``aws:TagKeys`  
Specify tag\-based conditions\. For details, see [Using tags to control access to Elastic Beanstalk resources](AWSHowTo.iam.policies.access-tags.md)\.