# Terminating HTTPS on EC2 instances running PHP<a name="https-singleinstance-php"></a>

For PHP container types, you use a [configuration file](ebextensions.md) to enable the Apache HTTP Server to use HTTPS\.

Add the following snippet to your configuration file, replacing the certificate and private key material as instructed, and save it in your source bundle's `.ebextensions` directory\.

The configuration file performs the following tasks:
+ The `packages` key uses yum to install `mod24_ssl`\.
+ The `files` key creates the following files on the instance:  
`/etc/httpd/conf.d/ssl.conf`  
Configures the Apache server\. This file loads when the Apache service starts\.  
`/etc/pki/tls/certs/server.crt`  
Creates the certificate file on the instance\. Replace *certificate file contents* with the contents of your certificate\.  
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.
If you have intermediate certificates, include them in `server.crt` after your site certificate\.  

  ```
        -----BEGIN CERTIFICATE-----
    certificate file contents
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    first intermediate certificate
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    second intermediate certificate
    -----END CERTIFICATE-----
  ```  
`/etc/pki/tls/certs/server.key`  
Creates the private key file on the instance\. Replace *private key contents* with the contents of the private key used to create the certificate request or self\-signed certificate\.

**Example \.ebextensions/https\-instance\.config**  

```
packages:
  yum:
    mod24_ssl : []

files:
  /etc/httpd/conf.d/ssl.conf:
    mode: "000644"
    owner: root
    group: root
    content: |
      LoadModule ssl_module modules/mod_ssl.so
      Listen 443
      <VirtualHost *:443>
        <Proxy *>
          Order deny,allow
          Allow from all
        </Proxy>

        SSLEngine             on
        SSLCertificateFile    "/etc/pki/tls/certs/server.crt"
        SSLCertificateKeyFile "/etc/pki/tls/certs/server.key"
        SSLCipherSuite        EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
        SSLProtocol           All -SSLv2 -SSLv3
        SSLHonorCipherOrder   On
        SSLSessionTickets     Off
        
        Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
        Header always set X-Frame-Options DENY
        Header always set X-Content-Type-Options nosniff
        
        ProxyPass / http://localhost:80/ retry=0
        ProxyPassReverse / http://localhost:80/
        ProxyPreserveHost on
        RequestHeader set X-Forwarded-Proto "https" early
        
      </VirtualHost>
      
  /etc/pki/tls/certs/server.crt:
    mode: "000400"
    owner: root
    group: root
    content: |
      -----BEGIN CERTIFICATE-----
      certificate file contents
      -----END CERTIFICATE-----
      
  /etc/pki/tls/certs/server.key:
    mode: "000400"
    owner: root
    group: root
    content: |
      -----BEGIN RSA PRIVATE KEY-----
      private key contents # See note below.
      -----END RSA PRIVATE KEY-----
```

**Note**  
Avoid committing a configuration file that contains your private key to source control\. After you have tested the configuration and confirmed that it works, store your private key in Amazon S3 and modify the configuration to download it during deployment\. For instructions, see [Storing private keys securely in Amazon S3](https-storingprivatekeys.md)\.

In a single instance environment, you must also modify the instance's security group to allow traffic on port 443\. The following configuration file retrieves the security group's ID using an AWS CloudFormation [function](ebextensions-functions.md) and adds a rule to it\.

**Example \.ebextensions/https\-instance\-single\.config**  

```
Resources:
  sslSecurityGroupIngress: 
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: {"Fn::GetAtt" : ["AWSEBSecurityGroup", "GroupId"]}
      IpProtocol: tcp
      ToPort: 443
      FromPort: 443
      CidrIp: 0.0.0.0/0
```

For a load\-balanced environment, you configure the load balancer to either [pass secure traffic through untouched](https-tcp-passthrough.md), or [decrypt and re\-encrypt](configuring-https-endtoend.md) for end\-to\-end encryption\.