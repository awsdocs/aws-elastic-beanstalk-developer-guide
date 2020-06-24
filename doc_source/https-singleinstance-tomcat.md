# Terminating HTTPS on EC2 instances running Tomcat<a name="https-singleinstance-tomcat"></a>

For Tomcat container types, you use a [configuration file](ebextensions.md) to enable the Apache HTTP Server to use HTTPS when acting as the reverse proxy for Tomcat\.

Add the following snippet to your configuration file, replacing the certificate and private key material as instructed, and save it in your source bundle's `.ebextensions` directory\. The configuration file performs the following tasks:
+ The `files` key creates the following files on the instance:  
`/etc/pki/tls/certs/server.crt`  
Creates the certificate file on the instance\. Replace *certificate file contents* with the contents of your certificate\.  
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.  
`/etc/pki/tls/certs/server.key`  
Creates the private key file on the instance\. Replace *private key contents* with the contents of the private key used to create the certificate request or self\-signed certificate\.   
`/opt/elasticbeanstalk/hooks/appdeploy/post/99_start_httpd.sh`  
Creates a post\-deployment hook script to restart the httpd service\.

**Example \.ebextensions/https\-instance\.config**  

```
files:
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

  /opt/elasticbeanstalk/hooks/appdeploy/post/99_start_httpd.sh:
    mode: "000755"
    owner: root
    group: root
    content: |
      #!/usr/bin/env bash
      sudo service httpd restart
```

You must also configure your environment's proxy server to listen on port 443\. The following Apache 2\.4 configuration adds a listener on port 443\. To learn more, see [Configuring your Tomcat environment's proxy server](java-tomcat-proxy.md)\.

**Example \.ebextensions/httpd/conf\.d/ssl\.conf**  

```
Listen 443
<VirtualHost *:443> 
  ServerName server-name
  SSLEngine on 
  SSLCertificateFile "/etc/pki/tls/certs/server.crt" 
  SSLCertificateKeyFile "/etc/pki/tls/certs/server.key" 

  <Proxy *> 
    Require all granted 
  </Proxy> 
  ProxyPass / http://localhost:8080/ retry=0 
  ProxyPassReverse / http://localhost:8080/ 
  ProxyPreserveHost on 

  ErrorLog /var/log/httpd/elasticbeanstalk-ssl-error_log 

</VirtualHost>
```

Your certificate vendor may include intermediate certificates that you can install for better compatibility with mobile clients\. Configure Apache with an intermediate certificate authority \(CA\) bundle by adding the following to your SSL configuration file \(see [Extending and overriding the default Apache configuration](java-tomcat-proxy.md#java-tomcat-proxy-apache) for the location\):
+ In the `ssl.conf` file contents, specify the chain file:

  ```
  SSLCertificateKeyFile "/etc/pki/tls/certs/server.key"
  SSLCertificateChainFile "/etc/pki/tls/certs/gd_bundle.crt"
  SSLCipherSuite        EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
  ```
+ Add a new entry to the `files` key with the contents of the intermediate certificates:

  ```
  files:
    /etc/pki/tls/certs/gd_bundle.crt:
      mode: "000400"
      owner: root
      group: root
      content: |
        -----BEGIN CERTIFICATE-----
        First intermediate certificate
        -----END CERTIFICATE-----
        -----BEGIN CERTIFICATE-----
        Second intermediate certificate
        -----END CERTIFICATE-----
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