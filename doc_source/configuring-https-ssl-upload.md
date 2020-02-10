# Upload a certificate to IAM<a name="configuring-https-ssl-upload"></a>

To use your certificate with your Elastic Beanstalk environment's load balancer, upload the certificate and private key to AWS Identity and Access Management \(IAM\)\. You can use a certificate stored in IAM with Elastic Load Balancing load balancers and Amazon CloudFront distributions\.

**Note**  
AWS Certificate Manager \(ACM\) is the preferred tool to provision, manage, and deploy your server certificates\. For more information about requesting an ACM certificate, see [Request a Certificate](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request.html) in the *AWS Certificate Manager User Guide*\. For more information about importing third\-party certificates into ACM, see [Importing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html) in the *AWS Certificate Manager User Guide*\. Use IAM to upload a certificate only if ACM is not [available in your AWS Region](https://docs.aws.amazon.com/general/latest/gr/acm.html)\.

You can use the [AWS Command Line Interface](chapter-devenv.md#devenv-awscli) \(AWS CLI\) to upload your certificate\. The following command uploads a self\-signed certificate named *https\-cert\.crt* with a private key named *private\-key\.pem*:

```
$ aws iam upload-server-certificate --server-certificate-name elastic-beanstalk-x509 --certificate-body file://https-cert.crt --private-key file://private-key.pem
{
    "ServerCertificateMetadata": {
        "ServerCertificateId": "AS5YBEIONO2Q7CAIHKNGC",
        "ServerCertificateName": "elastic-beanstalk-x509",
        "Expiration": "2017-01-31T23:06:22Z",
        "Path": "/",
        "Arn": "arn:aws:iam::123456789012:server-certificate/elastic-beanstalk-x509",
        "UploadDate": "2016-02-01T23:10:34.167Z"
    }
}
```

The `file://` prefix tells the AWS CLI to load the contents of a file in the current directory\. *elastic\-beanstalk\-x509* specifies the name to call the certificate in IAM\.

If you purchased a certificate from a certificate authority and received a certificate chain file, upload that as well by including the `--certificate-chain` option:

```
$ aws iam upload-server-certificate --server-certificate-name elastic-beanstalk-x509 --certificate-chain file://certificate-chain.pem --certificate-body file://https-cert.crt --private-key file://private-key.pem
```

Make note of the Amazon Resource Name \(ARN\) for your certificate\. You'll use it when you update your load balancer configuration settings to use HTTPS\.

**Note**  
A certificate uploaded to IAM stays stored even after it's no longer used in any environment's load balancer\. It contains sensitive data\. When you no longer need the certificate for any environment, be sure to delete it\. For details about deleting a certificate from IAM, see [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html#delete-server-certificate](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html#delete-server-certificate)\.

For more information about server certificates in IAM, see [Working with Server Certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html) in the *IAM User Guide*\.