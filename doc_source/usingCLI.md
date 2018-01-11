# Getting Set Up<a name="usingCLI"></a>

**Note**  
 This tool, the Elastic Beanstalk API CLI, and its documentation have been replaced with the AWS CLI\. See the [AWS CLI User Guide ](http://docs.aws.amazon.com/cli/latest/userguide/) to get started with the AWS CLI\. Also try the EB CLI for a simplified, higher level command line experience\. 

 Elastic Beanstalk provides a command line interface \(CLI\) to access Elastic Beanstalk functionality without using the AWS Management Console or the APIs\. This section describes the prerequisites for running the CLI tools \(or command line tools\), where to get the tools, how to set up the tools and their environment, and includes a series of common examples of tool usage\.

## Prerequisites<a name="prerequisites"></a>

 This document assumes you can work in a Linux/UNIX or Windows environment\. The Elastic Beanstalk command line interface also works correctly on Mac OS X \(which resembles the Linux and UNIX command environment\), but no specific Mac OS X instructions are included in this guide\. 

 As a convention, all command line text is prefixed with a generic **PROMPT> **command line prompt\. The actual command line prompt on your machine is likely to be different\. We also use **$ ** to indicate a Linux/UNIX\-specific command and **C:\\> ** for a Windows\-specific command\. The example output resulting from the command is shown immediately thereafter without any prefix\. 

 The command line tools used in this guide require Ruby \(version 1\.8\.7\+ or 1\.9\.2\+\) and Python version 2\.7 to run\. To view and download Ruby clients for a range of platforms, including Linux/UNIX and Windows, go to [ http://www\.ruby\-lang\.org/en/](http://www.ruby-lang.org/en/)\. Python is available at [python\.org](https://www.python.org/)\.

**Note**  
 If you are using Linux with a system version of Linux lower than 2\.7, install Python 2\.7 with your distribution's package manager and then modify the `eb` script under `eb/linux/python2.7/eb` to refer to the Python 2\.7 executable:   

```
#!/usr/bin/env python2.7
```

Additionally, you will need to install the `boto` module with `pip`:

```
$ sudo /usr/bin/easy_install-2.7 pip
$ sudo pip install boto
```

## Getting the Command Line Tools<a name="usingCLI.StartCLI.Getting"></a>

The command line tools are available as a \.zip file on the [AWS Sample Code & Libraries](http://aws.amazon.com/code/6752709412171743) website\. These tools are written in Ruby, and include shell scripts for Windows 2000, Windows XP, Windows Vista, Windows 7, Linux/UNIX, and Mac OS X\. The \.zip file is self\-contained and no installation is required; simply download the \.zip file and unzip it to a directory on your local machine\. You can find the tools in the **api** directory\.

## Providing Credentials for the Command Line Interface<a name="usingCLI.StartCLI.WhoYouAre"></a>

 The command line interface requires the access key ID and secret access key\. To get your access keys \(access key ID and secret access key\), see [How Do I Get Security Credentials?](http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *AWS General Reference*\.

You need to create a file containing your access key ID and secret access key\. The contents of the file should look like this:

```
AWSAccessKeyId=Write your AWS access ID
AWSSecretKey=Write your AWS secret key
```

**Important**  
On UNIX, limit permissions to the owner of the credential file:  

```
$ chmod 600 <the file created above>
```

With the credentials file set up, you'll need to set the AWS\_CREDENTIAL\_FILE environment variable so that the Elastic Beanstalk CLI tools can find your information\.

 **To set the AWS\_CREDENTIAL\_FILE environment variable** 

+ Set the environment variable using the following command:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/usingCLI.html)

## Set the Service Endpoint URL<a name="usingCLI.StartCLI.endpoints"></a>

 By default, the AWS Elastic Beanstalk uses the US East \(N\. Virginia\) region \(us\-east\-1\) with the elasticbeanstalk\.us\-east\-1\.amazonaws\.com service endpoint URL\. This section describes how to specify a different region by setting the service endpoint URL\. For information about this product's regions, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html?r=1166) in the Amazon Web Services General Reference\. 

**To set the service endpoint URL**

+ Set the environment variable using the following command:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/usingCLI.html)

  For example, on Linux, type the following to set your endpoint to us\-west\-2:

  ```
  export ELASTICBEANSTALK_URL="https://elasticbeanstalk.us-west-2.amazonaws.com" 
  ```

  For example, on Windows, type the following to set your endpoint to us\-west\-2:

  ```
  set ELASTICBEANSTALK_URL=https://elasticbeanstalk.us-west-2.amazonaws.com 
  ```