# Resources and Conditions for Elastic Beanstalk Actions<a name="AWSHowTo.iam.policies.actions"></a>

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"],
          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-west-2::solutionstack/32bit Amazon Linux running Tomcat 7"]
        }
      }
    }
  ]
}
```


+ [Policy Information for Elastic Beanstalk Actions](#AWSHowTo.iam.policies.actions.table)
+ [Condition Keys for Elastic Beanstalk Actions](#AWSHowTo.iam.policies.conditions)

## Policy Information for Elastic Beanstalk Actions<a name="AWSHowTo.iam.policies.actions.table"></a>

The following table lists all Elastic Beanstalk actions, the resource that each action acts upon, and the additional contextual information that can be provided using conditions\.


**Policy information for Elastic Beanstalk actions, including resources, conditions, examples, and dependencies**  

| Resource | Conditions | Example Statement | 
| --- | --- | --- | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_AbortEnvironmentUpdate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_AbortEnvironmentUpdate.html) | 
|  `application` `environment`  |  N/A  |  The following policy allows a user to abort environment update operations on environments in an application named `My App`\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:AbortEnvironmentUpdate"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CheckDNSAvailability.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CheckDNSAvailability.html) | 
|  `"*"`  |  N/A  |  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:CheckDNSAvailability"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ComposeEnvironments.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ComposeEnvironments.html) | 
|  `application`  |  N/A  |  The following policy allows a user to compose environments that belong to an application named `My App`\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:ComposeEnvironments"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplication.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplication.html) | 
|  `application`  |  N/A  |  This example allows the `CreateApplication` action to create applications whose names begin with **DivA**: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:CreateApplication"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:application/DivA*"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplicationVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateApplicationVersion.html) | 
|  `applicationversion`  |  `InApplication`  |  This example allows the `CreateApplicationVersion` action to create application versions with any name \(**\***\) in the application **My App**: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:CreateApplicationVersion"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/*"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateConfigurationTemplate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateConfigurationTemplate.html) | 
|  `configurationtemplate`  |  `InApplication` `FromApplication` `FromApplicationVersion` `FromConfigurationTemplate` `FromEnvironment` `FromSolutionStack`  |  The following policy allows the `CreateConfigurationTemplate` action to create configuration templates whose name begins with **My Template** \(`My Template*`\) in the application **My App**: 

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:configurationtemplate/My App/My Template*"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-west-2::solutionstack/32bit Amazon Linux running Tomcat 7"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateEnvironment.html) | 
|  `environment`  |  `InApplication` `FromApplicationVersion` `FromConfigurationTemplate` `FromSolutionStack`  |  The following policy allows the `CreateEnvironment` action to create an environment whose name is **myenv** in the application **My App** and using the solution stack **32bit Amazon Linux running Tomcat 7**: 

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"],
          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-west-2::solutionstack/32bit Amazon Linux running Tomcat 7"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateStorageLocation.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_CreateStorageLocation.html) | 
|  `"*"`  |  N/A  |  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:CreateStorageLocation"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplication.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplication.html) | 
|  `application`  |  N/A  |  The following policy allows the `DeleteApplication` action to delete the application **My App**: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:DeleteApplication"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplicationVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteApplicationVersion.html) | 
|  `applicationversion`  |  `InApplication`  |  The following policy allows the `DeleteApplicationVersion` action to delete an application version whose name is **My Version** in the application **My App**: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:DeleteApplicationVersion"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }        
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteConfigurationTemplate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteConfigurationTemplate.html) | 
|  `configurationtemplate`  |  `InApplication` \(Optional\)  |  The following policy allows the `DeleteConfigurationTemplate` action to delete a configuration template whose name is **My Template** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:DeleteConfigurationTemplate"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:configurationtemplate/My App/My Template"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteEnvironmentConfiguration.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DeleteEnvironmentConfiguration.html) | 
|  `environment`  |  `InApplication` \(Optional\)  |  The following policy allows the `DeleteEnvironmentConfiguration` action to delete a draft configuration for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:DeleteEnvironmentConfiguration"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplications.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplications.html) | 
|  `application`  |  N/A  |  The following policy allows the `DescribeApplications` action to describe the application My App\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:DescribeApplications"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplicationVersions.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeApplicationVersions.html) | 
|  `applicationversion`  |  `InApplication` \(Optional\)  |  The following policy allows the `DescribeApplicationVersions` action to describe the application version **My Version** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:DescribeApplicationVersions"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationOptions.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationOptions.html) | 
|  `environment` `configurationtemplate` `solutionstack`  |  `InApplication` \(Optional\)  |  The following policy allows the `DescribeConfigurationOptions` action to describe the configuration options for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeConfigurationOptions",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationSettings.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationSettings.html) | 
|  `environment`, `configurationtemplate`  |  `InApplication` \(Optional\)  |  The following policy allows the `DescribeConfigurationSettings` action to describe the configuration settings for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeConfigurationSettings",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentHealth.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentHealth.html) | 
|  `environment`  |  N/A  |  The following policy allows use of `DescribeEnvironmentHealth` to retrieve health information for an environment named **myenv**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeEnvironmentHealth",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentResources.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironmentResources.html) | 
|  `environment`  |  `InApplication` \(Optional\)  |  The following policy allows the `DescribeEnvironmentResources` action to return list of AWS resources for the environment **myenv** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeEnvironmentResources",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironments.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEnvironments.html) | 
|  `environment`  |  `InApplication` \(Optional\)  |  The following policy allows the `DescribeEnvironments` action to describe the environments **myenv** and **myotherenv** in the application **My App**\. Specifying the application name as a condition is optional\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeEnvironments",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv",
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App2/myotherenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEvents.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeEvents.html) | 
|  `application`  `applicationversion` `configurationtemplate` `environment`  |  `InApplication`  |  The following policy allows the `DescribeEvents` action to list event descriptions for the environment **myenv** and the application version **My Version** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeEvents",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv",
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeInstancesHealth.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeInstancesHealth.html) | 
|  `environment`  |  N/A  |  The following policy allows use of `DescribeInstancesHealth` to retrieve health information for instances in an environment named **myenv**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "elasticbeanstalk:DescribeInstancesHealth",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ListAvailableSolutionStacks.html) | 
|  `solutionstack`  |  N/A  |  The following policy allows the `ListAvailableSolutionStacks` action to return only the solution stack **32bit Amazon Linux running Tomcat 7**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:ListAvailableSolutionStacks"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:elasticbeanstalk:us-west-2::solutionstack/32bit Amazon Linux running Tomcat 7"
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RebuildEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RebuildEnvironment.html) | 
|  `environment`  |  `InApplication`  |  The following policy allows the `RebuildEnvironment` action to rebuild the environment **myenv** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:RebuildEnvironment"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RequestEnvironmentInfo.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RequestEnvironmentInfo.html) | 
|  `environment`  |  `InApplication`  |  The following policy allows the `RequestEnvironmentInfo` action to compile information about the environment **myenv** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:RequestEnvironmentInfo"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RestartAppServer.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RestartAppServer.html) | 
|  `environment`  |  `InApplication`  |  The following policy allows the `RestartAppServer` action to restart the application container server for the environment **myenv** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:RestartAppServer"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RetrieveEnvironmentInfo.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_RetrieveEnvironmentInfo.html) | 
|  `environment`  |  `InApplication`  |  The following policy allows the `RetrieveEnvironmentInfo` action to retrieve the compiled information for the environment **myenv** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:RetrieveEnvironmentInfo"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_SwapEnvironmentCNAMEs.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_SwapEnvironmentCNAMEs.html) | 
|  `environment`  |  `InApplication` \(Optional\) `FromEnvironment` \(Optional\)  |  The following policy allows the `SwapEnvironmentCNAMEs` action to swap the CNAMEs for the environments **mysrcenv** and **mydestenv**\.  

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/mysrcenv",
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/mydestenv"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html) | 
|  `environment`  |  `InApplication`  |  The following policy allows the `TerminateEnvironment` action to terminate the environment **myenv** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:TerminateEnvironment"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[UpdateApplication](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplication.html) | 
|  `application`  |  N/A  |  The following policy allows the `UpdateApplication` action to update properties of the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:UpdateApplication"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"
      ]
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplicationVersion.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateApplicationVersion.html) | 
|  `applicationversion`  |  `InApplication`  |  The following policy allows the `UpdateApplicationVersion` action to update the properties of the application version **My Version** in the application **My App**\. 

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateConfigurationTemplate.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateConfigurationTemplate.html) | 
|  `configurationtemplate`  |  `InApplication`  |  The following policy allows the `UpdateConfigurationTemplate` action to update the properties or options of the configuration template **My Template** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:UpdateConfigurationTemplate"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:configurationtemplate/My App/My Template"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateEnvironment.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateEnvironment.html) | 
|  `environment`  |  `InApplication` `FromApplicationVersion` `FromConfigurationTemplate`  |  The following policy allows the `UpdateEnvironment` action to update the environment **myenv** in the application **My App** by deploying the application version **My Version**\. 

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"]
        }
      }
    }
  ]
}
```  | 
| **Action: **[http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ValidateConfigurationSettings.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_ValidateConfigurationSettings.html) | 
|  `template` `environment`  |  `InApplication`  |  The following policy allows the `ValidateConfigurationSettings` action to validates configuration settings against the environment **myenv** in the application **My App**\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "elasticbeanstalk:ValidateConfigurationSettings"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
        }
      }
    }
  ]
}
```  | 

## Condition Keys for Elastic Beanstalk Actions<a name="AWSHowTo.iam.policies.conditions"></a>

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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"]
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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:applicationversion/My App/My Version"]
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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/myenv"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromConfigurationTemplate": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:configurationtemplate/My App/My Template"]
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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/mysrcenv*",
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/mydestenv*"
      ],
      "Condition": {
        "StringNotLike": {
          "elasticbeanstalk:FromEnvironment": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/mysrcenvPROD*"],
          "elasticbeanstalk:FromEnvironment": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:environment/My App/mydestenvPROD*"
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
        "arn:aws:elasticbeanstalk:us-west-2:123456789012:configurationtemplate/My App/My Template*"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-west-2:123456789012:application/My App"],
          "elasticbeanstalk:FromSolutionStack": ["arn:aws:elasticbeanstalk:us-west-2::solutionstack/32bit Amazon Linux running Tomcat 7"]
        }
      }
    }
  ]
}
```