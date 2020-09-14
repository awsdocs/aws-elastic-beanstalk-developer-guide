# Create and sign an X509 certificate<a name="configuring-https-ssl"></a>

You can create an X509 certificate for your application with `OpenSSL`\. OpenSSL is a standard, open source library that supports a wide range of cryptographic functions, including the creation and signing of x509 certificates\. For more information about OpenSSL, visit [www\.openssl\.org](https://www.openssl.org/)\.

**Note**  
You only need to create a certificate locally if you want to [use HTTPS in a single instance environment](https-singleinstance.md) or [re\-encrypt on the backend](configuring-https-endtoend.md) with a self\-signed certificate\. If you own a domain name, you can create a certificate in AWS and use it with a load\-balanced environment for free by using AWS Certificate Manager \(ACM\)\. See [Request a Certificate](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request.html) in the *AWS Certificate Manager User Guide* for instructions\.

Run `openssl version` at the command line to see if you already have OpenSSL installed\. If you don't, you can build and install the source code using the instructions at the [public GitHub repository](https://github.com/openssl/openssl), or use your favorite package manager\. OpenSSL is also installed on Elastic Beanstalk's Linux images, so a quick alternative is to connect to an EC2 instance in a running environment by using the [EB CLI](eb-cli3.md)'s eb ssh command:

```
~/eb$ eb ssh
[ec2-user@ip-255-55-55-255 ~]$ openssl version
OpenSSL 1.0.1k-fips 8 Jan 2015
```

You need to create an RSA private key to create your certificate signing request \(CSR\)\. To create your private key, use the openssl genrsa command:

```
[ec2-user@ip-255-55-55-255 ~]$ openssl genrsa 2048 > privatekey.pem
Generating RSA private key, 2048 bit long modulus
.................................................................................................................................+++
...............+++
e is 65537 (0x10001)
```

*privatekey\.pem*  
The name of the file where you want to save the private key\. Normally, the openssl genrsa command prints the private key contents to the screen, but this command pipes the output to a file\. Choose any file name, and store the file in a secure place so that you can retrieve it later\. If you lose your private key, you won't be able to use your certificate\.

A CSR is a file you send to a certificate authority \(CA\) to apply for a digital server certificate\. To create a CSR, use the openssl req command:

```
$ openssl req -new -key privatekey.pem -out csr.pem
You are about to be asked to enter information that will be incorporated 
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
```

Enter the information requested and press **Enter**\. The following table describes and shows examples for each field\.


****  

| Name | Description | Example | 
| --- | --- | --- | 
| Country Name | The two\-letter ISO abbreviation for your country\. | US = United States | 
| State or Province | The name of the state or province where your organization is located\. You cannot abbreviate this name\. | Washington | 
| Locality Name | The name of the city where your organization is located\. | Seattle | 
| Organization Name | The full legal name of your organization\. Do not abbreviate your organization name\. | Example Corporation | 
| Organizational Unit | Optional, for additional organization information\. | Marketing | 
| Common Name | The fully qualified domain name for your web site\. This must match the domain name that users see when they visit your site, otherwise certificate errors will be shown\. | www\.example\.com | 
| Email address | The site administrator's email address\. | someone@example\.com | 

You can submit the signing request to a third party for signing, or sign it yourself for development and testing\. Self\-signed certificates can also be used for backend HTTPS between a load balancer and EC2 instances\.

To sign the certificate, use the openssl x509 command\. The following example uses the private key from the previous step \(*privatekey\.pem*\) and the signing request \(*csr\.pem*\) to create a public certificate named *public\.crt* that is valid for *365* days\.

```
$ openssl x509 -req -days 365 -in csr.pem -signkey privatekey.pem -out public.crt
Signature ok
subject=/C=us/ST=washington/L=seattle/O=example corporation/OU=marketing/CN=www.example.com/emailAddress=someone@example.com
Getting Private key
```

Keep the private key and public certificate for later use\. You can discard the signing request\. Always [store the private key in a secure location](https-storingprivatekeys.md) and avoid adding it to your source code\.

To use the certificate with the Windows Server platform, you must convert it to a PFX format\. Use the following command to create a PFX certificate from the private key and public certificate files:

```
$ openssl pkcs12 -export -out example.com.pfx -inkey privatekey.pem -in public.crt
Enter Export Password: password
Verifying - Enter Export Password: password
```

Now that you have a certificate, you can [upload it to IAM](configuring-https-ssl-upload.md) for use with a load balancer, or [configure the instances in your environment to terminate HTTPS](https-singleinstance.md)\.