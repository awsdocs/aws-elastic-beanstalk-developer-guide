# Creating Elastic Beanstalk environments with the AWS CLI<a name="environments-create-awscli"></a>

1. Check if the CNAME for the environment is available\.

   ```
   $ aws elasticbeanstalk check-dns-availability --cname-prefix my-cname
   {
       "Available": true,
       "FullyQualifiedCNAME": "my-cname.elasticbeanstalk.com"
   }
   ```

1. Make sure your application version exists\.

   ```
   $ aws elasticbeanstalk describe-application-versions --application-name my-app --version-label v1
   ```

   If you don't have an application version for your source yet, create it\. For example, the following command creates an application version from a source bundle in Amazon Simple Storage Service \(Amazon S3\)\.

   ```
   $ aws elasticbeanstalk create-application-version --application-name my-app --version-label v1 --source-bundle S3Bucket=my-bucket,S3Key=my-source-bundle.zip
   ```

1. Create a configuration template for the application\.

   ```
   $ aws elasticbeanstalk create-configuration-template --application-name my-app --template-name v1 --solution-stack-name "64bit Amazon Linux 2015.03 v2.0.0 running Ruby 2.2 (Passenger Standalone)"
   ```

1. Create environment\.

   ```
   $ aws elasticbeanstalk create-environment --cname-prefix my-cname --application-name my-app --template-name v1 --version-label v1 --environment-name v1clone --option-settings file://options.txt
   ```

   Option Settings are defined in the **options\.txt** file:

   ```
   [
       {
           "Namespace": "aws:autoscaling:launchconfiguration",
           "OptionName": "IamInstanceProfile",
           "Value": "aws-elasticbeanstalk-ec2-role"
       }
   ]
   ```

   The above option setting defines the IAM instance profile\. You can specify the ARN or the profile name\.

1. Determine if the new environment is Green and Ready\.

   ```
   $ aws elasticbeanstalk describe-environments --environment-names my-env
   ```

   If the new environment does not come up Green and Ready, you should decide if you want to retry the operation or leave the environment in its current state for investigation\. Make sure to terminate the environment after you are finished, and clean up any unused resources\.
**Note**  
You can adjust the timeout period if the environment doesn't launch in a reasonable time\.