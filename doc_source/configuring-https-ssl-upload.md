# Upload a Certificate to IAM<a name="configuring-https-ssl-upload"></a>

To use your certificate with your Elastic Beanstalk environment's load balancer, upload the certificate and private key to AWS Identity and Access Management \(IAM\)\. You can use a certificate stored in IAM with Elastic Load Balancing load balancers and CloudFront distributions\.

You can use the AWS Command Line Interface \(AWS CLI\) to upload your certificate\. The following command uploads a self\-signed certificate named *https\-cert\.crt* with a private key named *private\-key\.pem*:

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

Make note of the Amazon Resource Name \(ARN\) for your certificate\. You will use it when you update your load balancer configuration settings to use HTTPS\.

To see sample certificates that are valid with IAM, go to [Sample Certificates](http://docs.aws.amazon.com/IAM/latest/UserGuide/InstallCert.html) in the *AWS Identity and Access Management Using IAM User Guide*\.