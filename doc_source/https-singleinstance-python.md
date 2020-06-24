# Terminating HTTPS on EC2 instances running Python<a name="https-singleinstance-python"></a>

For Python container types using Apache HTTP Server with the Web Server Gateway Interface \(WSGI\), you use a [configuration file](ebextensions.md) to enable the Apache HTTP Server to use HTTPS\.

Add the following snippet to your [configuration file](ebextensions.md), replacing the certificate and private key material as instructed, and save it in your source bundle's `.ebextensions` directory\. The configuration file performs the following tasks:
+ The `packages` key uses yum to install `mod24_ssl`\.
+ The `files` key creates the following files on the instance:  
`/etc/httpd/conf.d/ssl.conf`  
Configures the Apache server\. If your application is not named `application.py`, replace the highlighted text in the value for `WSGIScriptAlias` with the local path to your application\. For example, a django application's may be at `django/wsgi.py`\. The location should match the value of the `WSGIPath` option that you set for your environment\.  
Depending on your application requirements, you may also need to add other directories to the python\-path parameter\.   
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
+ The `container_commands` key stops the httpd service after everything has been configured so that the service uses the new `https.conf` file and certificate\.

**Note**  
The example works only in environments using the [Python](create-deploy-python-container.md) platform\.

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
      LoadModule wsgi_module modules/mod_wsgi.so
      WSGIPythonHome /opt/python/run/baselinenv
      WSGISocketPrefix run/wsgi
      WSGIRestrictEmbedded On
      Listen 443
      <VirtualHost *:443>
        SSLEngine on
        SSLCertificateFile "/etc/pki/tls/certs/server.crt"
        SSLCertificateKeyFile "/etc/pki/tls/certs/server.key"
        
        Alias /static/ /opt/python/current/app/static/
        <Directory /opt/python/current/app/static>
        Order allow,deny
        Allow from all
        </Directory>
        
        WSGIScriptAlias / /opt/python/current/app/application.py
        
        <Directory /opt/python/current/app>
        Require all granted
        </Directory>
        
        WSGIDaemonProcess wsgi-ssl processes=1 threads=15 display-name=%{GROUP} \
          python-path=/opt/python/current/app \
          python-home=/opt/python/run/venv \
          home=/opt/python/current/app \
          user=wsgi \
          group=wsgi
        WSGIProcessGroup wsgi-ssl
        
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
      
container_commands:
  01killhttpd:
    command: "killall httpd"
  02waitforhttpddeath:
    command: "sleep 3"
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