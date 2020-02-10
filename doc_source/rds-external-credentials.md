# Storing the connection string in Amazon S3<a name="rds-external-credentials"></a>

Providing connection information to your application with environment properties is a good way to keep passwords out of your code, but it's not a perfect solution\. Environment properties are discoverable in the [environment management console](environments-console.md), and can be viewed by any user that has permission to [describe configuration settings](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationSettings.html) on your environment\. Depending on the platform, environment properties may also appear in [instance logs](using-features.logging.md)\.

You can lock down your connection information by storing it in an Amazon S3 bucket that you control\. The basic steps are as follows:
+ Upload a file that contains your connection string to an Amazon S3 bucket\.
+ Grant the EC2 instance profile permission to read the file\.
+ Configure your application to download the file during deployment\.
+ Read the file in your application code\.

First, create a bucket to store the file that contains your connection string\. For this example, we will use a JSON file that has a single key and value\. The value is a JDBC connection string for a PostgreSQL DB instance in Amazon RDS\.

`beanstalk-database.json`

```
{
  "connection": "jdbc:postgresql://mydb.b5uacpxznijm.us-west-2.rds.amazonaws.com:5432/ebdb?user=username&password=mypassword"
}
```

The highlighted portions of the URL correspond to the endpoint, port, DB name, user name and password for the database\.

**To create a bucket and upload a file**

1. Open the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.

1. Choose **Create Bucket**\.

1. Type a **Bucket Name**, and then choose a **Region**\.

1. Choose **Create**\.

1. Open the bucket, and then choose **Upload**

1. Follow the prompts to upload the file\.

By default, your account owns the file and has permission to manage it, but IAM users and roles do not unless you grant them access explicitly\. Grant the instances in your Elastic Beanstalk environment by adding a policy to the [instance profile](concepts-roles-instance.md)\.

The default instance profile is named `aws-elasticbeanstalk-ec2-role`\. If you're not sure what your instance profile is named, you can find it on the **Configuration** page in the [environment management console](using-features.managing.ec2.md)\.

**To add permissions to the instance profile**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home)\.

1. Choose **Roles**\.

1. Choose **aws\-elasticbeanstalk\-ec2\-role**\.

1. Choose **Add inline policy**\.

1. Add a policy that allows the instance to retrieve the file\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "database",
               "Action": [
                   "s3:GetObject"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:s3:::my-secret-bucket-123456789012/beanstalk-database.json"
               ]
           }
       ]
   }
   ```

   Replace the bucket and object names with the names of your bucket and object\.

Next, add a [configuration file](ebextensions.md) to your source code that tells Elastic Beanstalk to download the file from Amazon S3 during deployment\.

`~/my-app/.ebextensions/database.config`

```
Resources:
  AWSEBAutoScalingGroup:
    Metadata:
      AWS::CloudFormation::Authentication:
        S3Auth:
          type: "s3"
          buckets: ["my-secret-bucket-123456789012"]
          roleName: "aws-elasticbeanstalk-ec2-role"

files:
  "/tmp/beanstalk-database.json" :
    mode: "000644"
    owner: root
    group: root
    authentication: "S3Auth"
    source: https://s3-us-west-2.amazonaws.com/my-secret-bucket-123456789012/beanstalk-database.json
```

This configuration file does two things\. The `Resources` key adds an authentication method to your environment's Auto Scaling group metadata that Elastic Beanstalk can use to access Amazon S3\. The `files` key tells Elastic Beanstalk to download the file from Amazon S3 and store it locally in `/tmp/` during deployment\.

Deploy your application with the configuration file in `.ebextensions` folder at the root of your source code\. If you configured permissions correctly, the deployment will succeed and the file will be downloaded to all of the instances in your environment\. If not, the deployment will fail\.

Finally, add code to your application to read the JSON file and use the connection string to connect to the database\.