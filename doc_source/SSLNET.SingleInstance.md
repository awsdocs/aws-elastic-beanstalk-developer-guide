# Terminating HTTPS on Amazon EC2 instances running \.NET<a name="SSLNET.SingleInstance"></a>

The following [configuration file](ebextensions.md) creates and runs a Windows PowerShell script that performs the following tasks:
+ Checks for an existing HTTPS certificate binding to port 443
+ Gets the [PFX certificate](configuring-https-ssl.md) and password from an Amazon S3 bucket
**Note**  
Add an `AmazonS3ReadOnlyAccess` policy to the `aws-elasticbeanstalk-ec2-role` to access the SSL certificate and password files in the Amazon S3 bucket\.
+ Installs the certificate
+ Binds the certificate to port 443
**Note**  
To remove the HTTP endpoint \(port 80\), include the `Remove-WebBinding` command under the **Remove the HTTP binding** section of the example\.

**Example \.ebextensions/https\-instance\-dotnet\.config**  

```
files:
  "C:\\certs\\install-cert.ps1":
    content: |
      import-module webadministration
      ## Settings - replace the following values with your own
      $bucket = "my-bucket"        ## S3 bucket name
      $certkey = "example.com.pfx" ## S3 object key for your PFX certificate
      $pwdkey = "password.txt"     ## S3 object key for a text file containing the certificate's password
      ##

      # Set variables
      $certfile = "C:\cert.pfx"
      $pwdfile = "C:\certs\pwdcontent"
      Read-S3Object -BucketName $bucket -Key $pwdkey -File $pwdfile
      $pwd = Get-Content $pwdfile -Raw

      # Clean up existing binding
      if ( Get-WebBinding "Default Web Site" -Port 443 ) {
        Echo "Removing WebBinding"
        Remove-WebBinding -Name "Default Web Site" -BindingInformation *:443:
      }
      if ( Get-Item -path IIS:\SslBindings\0.0.0.0!443 ) {
        Echo "Deregistering WebBinding from IIS"
        Remove-Item -path IIS:\SslBindings\0.0.0.0!443
      }

      # Download certificate from S3
      Read-S3Object -BucketName $bucket -Key $certkey -File $certfile
      
      # Install certificate
      Echo "Installing cert..."
      $securepwd = ConvertTo-SecureString -String $pwd -Force -AsPlainText
      $cert = Import-PfxCertificate -FilePath $certfile cert:\localMachine\my -Password $securepwd
      
      # Create site binding
      Echo "Creating and registering WebBinding"
      New-WebBinding -Name "Default Web Site" -IP "*" -Port 443 -Protocol https
      New-Item -path IIS:\SslBindings\0.0.0.0!443 -value $cert -Force
      
      ## Remove the HTTP binding
      ## (optional) Uncomment the following line to unbind port 80
      # Remove-WebBinding -Name "Default Web Site" -BindingInformation *:80:
      ##
      
      # Update firewall
      netsh advfirewall firewall add rule name="Open port 443" protocol=TCP localport=443 action=allow dir=OUT

commands:
  00_install_ssl:
    command: powershell -NoProfile -ExecutionPolicy Bypass -file C:\\certs\\install-cert.ps1
```

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