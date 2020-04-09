# Storing private keys securely in Amazon S3<a name="https-storingprivatekeys"></a>

The private key that you use to sign your public certificate is private and should not be committed to source code\. You can avoid storing private keys in configuration files by uploading them to Amazon S3, and configuring Elastic Beanstalk to download the file from Amazon S3 during application deployment\.

The following example shows the [Resources](environment-resources.md) and [files](customize-containers-ec2.md#linux-files) sections of a [configuration file](ebextensions.md) downloads a private key file from an Amazon S3 bucket\.

**Example \.ebextensions/privatekey\.config**  

```
Resources:
  AWSEBAutoScalingGroup:
    Metadata:
      AWS::CloudFormation::Authentication:
        S3Auth:
          type: "s3"
          buckets: ["elasticbeanstalk-us-west-2-123456789012"]
          roleName: 
            "Fn::GetOptionSetting": 
              Namespace: "aws:autoscaling:launchconfiguration"
              OptionName: "IamInstanceProfile"
              DefaultValue: "aws-elasticbeanstalk-ec2-role"
files:
  # Private key
  "/etc/pki/tls/certs/server.key":
    mode: "000400"
    owner: root
    group: root
    authentication: "S3Auth"
    source: https://elasticbeanstalk-us-west-2-123456789012.s3.us-west-2.amazonaws.com/server.key
```

Replace the bucket name and URL in the example with your own\. The first entry in this file adds an authentication method named `S3Auth` to the environment's Auto Scaling group's metadata\. If you have configured a custom [instance profile](concepts-roles-instance.md) for your environment, that will be used, otherwise the default value of `aws-elasticbeanstalk-ec2-role` is applied\. The default instance profile has permission to read from the Elastic Beanstalk storage bucket\. If you use a different bucket, [add permissions to the instance profile](iam-instanceprofile.md#iam-instanceprofile-addperms)\.

The second entry uses the `S3Auth` authentication method to download the private key from the specified URL and save it to `/etc/pki/tls/certs/server.key`\. The proxy server can then read the private key from this location to [terminate HTTPS connections at the instance](https-singleinstance.md)\.

The instance profile assigned to your environment's EC2 instances must have permission to read the key object from the specified bucket\. [Verify that the instance profile has permission](iam-instanceprofile.md#iam-instanceprofile-verify) to read the object in IAM, and that the permissions on the bucket and object do not prohibit the instance profile\.

**To view a bucket's permissions**

1. Open the [Amazon S3 Management Console](https://console.aws.amazon.com/s3/home)\.

1. Choose a bucket\.

1. Choose **Properties** and then choose **Permissions**\.

1. Verify that your account is a grantee on the bucket with read permission\.

1. If a bucket policy is attached, choose **Bucket policy** to view the permissions assigned to the bucket\.