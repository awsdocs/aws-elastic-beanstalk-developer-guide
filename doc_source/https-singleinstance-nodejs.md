# Terminating HTTPS on EC2 instances running Node\.js<a name="https-singleinstance-nodejs"></a>

The following example configuration file [extends the default nginx configuration](nodejs-platform-proxy.md) to listen on port 443 and terminate SSL/TLS connections with a public certificate and private key\.

If you configured your environment for [enhanced health reporting](health-enhanced.md), you need to configure nginx to generate access logs\. To do that, uncomment the block of lines under the comment that reads `# For enhanced health...` by removing the leading `#` characters\.

**Example \.ebextensions/https\-instance\.config**  

```
files:
  /etc/nginx/conf.d/https.conf:
    mode: "000644"
    owner: root
    group: root
    content: |
      # HTTPS server

      server {
          listen       443;
          server_name  localhost;
          
          ssl                  on;
          ssl_certificate      /etc/pki/tls/certs/server.crt;
          ssl_certificate_key  /etc/pki/tls/certs/server.key;
          
          ssl_session_timeout  5m;
          
          ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
          ssl_prefer_server_ciphers   on;

          # For enhanced health reporting support, uncomment this block:

          #if ($time_iso8601 ~ "^(\d{4})-(\d{2})-(\d{2})T(\d{2})") {
          #    set $year $1;
          #    set $month $2;
          #    set $day $3;
          #    set $hour $4;
          #}
          #access_log /var/log/nginx/healthd/application.log.$year-$month-$day-$hour healthd;
          #access_log  /var/log/nginx/access.log  main;
          
          location / {
              proxy_pass  http://nodejs;
              proxy_set_header   Connection "";
              proxy_http_version 1.1;
              proxy_set_header        Host            $host;
              proxy_set_header        X-Real-IP       $remote_addr;
              proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
              proxy_set_header        X-Forwarded-Proto https;
          }
      }
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

The `files` key creates the following files on the instance:

`/etc/nginx/conf.d/https.conf`  
Configures the nginx server\. This file is loaded when the nginx service starts\.

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