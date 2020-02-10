# Creating Elastic Beanstalk environments with the API<a name="environments-create-api"></a>

1. Call `CheckDNSAvailability` with the following parameter:
   + `CNAMEPrefix` = `SampleApp`  
**Example**  

   ```
   1. https://elasticbeanstalk.us-east-2.amazonaws.com/?CNAMEPrefix=sampleapplication
   2. &Operation=CheckDNSAvailability
   3. &AuthParams
   ```

1. Call `DescribeApplicationVersions` with the following parameters:
   + `ApplicationName` = `SampleApp`
   + `VersionLabel` = `Version2`  
**Example**  

   ```
   1. https://elasticbeanstalk.us-east-2.amazonaws.com/?ApplicationName=SampleApp
   2. &VersionLabel=Version2
   3. &Operation=DescribeApplicationVersions
   4. &AuthParams
   ```

1. Call `CreateConfigurationTemplate` with the following parameters:
   + `ApplicationName` = `SampleApp`
   + `TemplateName` = `MyConfigTemplate`
   + `SolutionStackName` = `64bit%20Amazon%20Linux%202015.03%20v2.0.0%20running%20Ruby%202.2%20(Passenger%20Standalone)`  
**Example**  

   ```
   1. https://elasticbeanstalk.us-east-2.amazonaws.com/?ApplicationName=SampleApp
   2. &TemplateName=MyConfigTemplate
   3. &Operation=CreateConfigurationTemplate
   4. &SolutionStackName=64bit%20Amazon%20Linux%202015.03%20v2.0.0%20running%20Ruby%202.2%20(Passenger%20Standalone)
   5. &AuthParams
   ```

1. Call `CreateEnvironment` with one of the following sets of parameters\.

   1. Use the following for a web server environment tier:
      + `EnvironmentName` = `SampleAppEnv2`
      + `VersionLabel` = `Version2`
      + `Description` = `description`
      + `TemplateName` = `MyConfigTemplate`
      + `ApplicationName` = `SampleApp`
      + `CNAMEPrefix` = `sampleapplication`
      + `OptionSettings.member.1.Namespace` = `aws:autoscaling:launchconfiguration`
      + `OptionSettings.member.1.OptionName` = `IamInstanceProfile`
      + `OptionSettings.member.1.Value` = `aws-elasticbeanstalk-ec2-role`  
**Example**  

      ```
       1. https://elasticbeanstalk.us-east-2.amazonaws.com/?ApplicationName=SampleApp
       2. &VersionLabel=Version2
       3. &EnvironmentName=SampleAppEnv2
       4. &TemplateName=MyConfigTemplate
       5. &CNAMEPrefix=sampleapplication
       6. &Description=description
       7. &Operation=CreateEnvironment
       8. &OptionSettings.member.1.Namespace=aws%3Aautoscaling%3Alaunchconfiguration
       9. &OptionSettings.member.1.OptionName=IamInstanceProfile
      10. &OptionSettings.member.1.Value=aws-elasticbeanstalk-ec2-role
      11. &AuthParams
      ```

   1. Use the following for a worker environment tier:
      + `EnvironmentName` = `SampleAppEnv2`
      + `VersionLabel` = `Version2`
      + `Description` = `description`
      + `TemplateName` = `MyConfigTemplate`
      + `ApplicationName` = `SampleApp`
      + `Tier` = `Worker`
      + `OptionSettings.member.1.Namespace` = `aws:autoscaling:launchconfiguration`
      + `OptionSettings.member.1.OptionName` = `IamInstanceProfile`
      + `OptionSettings.member.1.Value` = `aws-elasticbeanstalk-ec2-role`
      + `OptionSettings.member.2.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.2.OptionName` = `WorkerQueueURL`
      + `OptionSettings.member.2.Value` = `sqsd.elasticbeanstalk.us-east-2.amazonaws.com`
      + `OptionSettings.member.3.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.3.OptionName` = `HttpPath`
      + `OptionSettings.member.3.Value` = `/`
      + `OptionSettings.member.4.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.4.OptionName` = `MimeType`
      + `OptionSettings.member.4.Value` = `application/json`
      + `OptionSettings.member.5.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.5.OptionName` = `HttpConnections`
      + `OptionSettings.member.5.Value` = `75`
      + `OptionSettings.member.6.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.6.OptionName` = `ConnectTimeout`
      + `OptionSettings.member.6.Value` = `10`
      + `OptionSettings.member.7.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.7.OptionName` = `InactivityTimeout`
      + `OptionSettings.member.7.Value` = `10`
      + `OptionSettings.member.8.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.8.OptionName` = `VisibilityTimeout`
      + `OptionSettings.member.8.Value` = `60`
      + `OptionSettings.member.9.Namespace` = `aws:elasticbeanstalk:sqsd`
      + `OptionSettings.member.9.OptionName` = `RetentionPeriod`
      + `OptionSettings.member.9.Value` = `345600`  
**Example**  

      ```
       1. https://elasticbeanstalk.us-east-2.amazonaws.com/?ApplicationName=SampleApp
       2. &VersionLabel=Version2
       3. &EnvironmentName=SampleAppEnv2
       4. &TemplateName=MyConfigTemplate
       5. &Description=description
       6. &Tier=Worker
       7. &Operation=CreateEnvironment
       8. &OptionSettings.member.1.Namespace=aws%3Aautoscaling%3Alaunchconfiguration
       9. &OptionSettings.member.1.OptionName=IamInstanceProfile
      10. &OptionSettings.member.1.Value=aws-elasticbeanstalk-ec2-role
      11. &OptionSettings.member.2.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      12. &OptionSettings.member.2.OptionName=WorkerQueueURL
      13. &OptionSettings.member.2.Value=sqsd.elasticbeanstalk.us-east-2.amazonaws.com
      14. &OptionSettings.member.3.Namespace=aws%3elasticbeanstalk%3sqsd
      15. &OptionSettings.member.3.OptionName=HttpPath
      16. &OptionSettings.member.3.Value=%2F
      17. &OptionSettings.member.4.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      18. &OptionSettings.member.4.OptionName=MimeType
      19. &OptionSettings.member.4.Value=application%2Fjson
      20. &OptionSettings.member.5.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      21. &OptionSettings.member.5.OptionName=HttpConnections
      22. &OptionSettings.member.5.Value=75
      23. &OptionSettings.member.6.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      24. &OptionSettings.member.6.OptionName=ConnectTimeout
      25. &OptionSettings.member.6.Value=10
      26. &OptionSettings.member.7.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      27. &OptionSettings.member.7.OptionName=InactivityTimeout
      28. &OptionSettings.member.7.Value=10
      29. &OptionSettings.member.8.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      30. &OptionSettings.member.8.OptionName=VisibilityTimeout
      31. &OptionSettings.member.8.Value=60
      32. &OptionSettings.member.9.Namespace=aws%3Aelasticbeanstalk%3Asqsd
      33. &OptionSettings.member.9.OptionName=RetentionPeriod
      34. &OptionSettings.member.9.Value=345600
      35. &AuthParams
      ```