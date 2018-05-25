# Amazon Resource Name Format for Elastic Beanstalk<a name="AWSHowTo.iam.policies.arn"></a>

You specify a resource for an IAM policy using that resource's Amazon Resource Name \(ARN\)\. For Elastic Beanstalk, the ARN has the following format\.

```
arn:aws:elasticbeanstalk:region:accountid:resourcetype/resourcepath
```

Where:
+ `region` is the region the resource resides in \(for example, **us\-west\-2**\)\.
+ `accountid` is the AWS account ID, with no hyphens \(for example, **123456789012**\)
+ `resourcetype` identifies the type of the Elastic Beanstalk resource—for example, `environment`\. See the table below for a list of all Elastic Beanstalk resource types\.
+ `resourcepath` is the portion that identifies the specific resource\. An Elastic Beanstalk resource has a path that uniquely identifies that resource\. See the table below for the format of the resource path for each resource type\. For example, an environment is always associated with an application\. The resource path for the environment **myEnvironment** in the application **myApp** would look like this:

  ```
  myApp/myEnvironment
  ```

Elastic Beanstalk has several types of resources you can specify in a policy\. The following table shows the ARN format for each resource type and an example\.


****  

| Resource Type | Format for ARN | 
| --- | --- | 
|  `application`  |  `arn:aws:elasticbeanstalk:region:accountid:application/applicationname` Example: **arn:aws:elasticbeanstalk:us\-east\-2:123456789012:application/My App**   | 
|  `applicationversion`  |  `arn:aws:elasticbeanstalk:region:accountid:applicationversion/applicationname/versionlabel` Example: **arn:aws:elasticbeanstalk:us\-east\-2:123456789012:applicationversion/My App/My Version**   | 
|  `configurationtemplate`  |  `arn:aws:elasticbeanstalk:region:accountid:configurationtemplate/applicationname/templatename` Example: **arn:aws:elasticbeanstalk:us\-east\-2:123456789012:configurationtemplate/My App/My Template**   | 
|  `environment`  |  `arn:aws:elasticbeanstalk:region:accountid:environment/applicationname/environmentname` Example: **arn:aws:elasticbeanstalk:us\-east\-2:123456789012:environment/My App/MyEnvironment**   | 
|  `platform`  |  `arn:aws:elasticbeanstalk:REGION:ACCOUNT_ID:platform/PLATFORM_NAME/PLATFORM_VERSION` Example: **arn:aws:elasticbeanstalk:us\-east\-2:123456789:platform/MyPlatform/1\.0**   | 
|  `solutionstack`  |  `arn:aws:elasticbeanstalk:region::solutionstack/solutionstackname` Example: **arn:aws:elasticbeanstalk:us\-east\-2::solutionstack/32bit Amazon Linux running Tomcat 7**   | 

An environment, application version, and configuration template are always contained within a specific application\. You'll notice that these resources all have an application name in their resource path so that they are uniquely identified by their resource name and the containing application\. Although solution stacks are used by configuration templates and environments, solution stacks are not specific to an application or AWS account and do not have the application or AWS account in their ARNs\.