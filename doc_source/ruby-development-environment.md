# Setting up your Ruby development environment<a name="ruby-development-environment"></a>

Set up a Ruby development environment to test your application locally prior to deploying it to AWS Elastic Beanstalk\. This topic outlines development environment setup steps and links to installation pages for useful tools\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

For common setup steps and tools that apply to all languages, see [Configuring your development machine for use with Elastic Beanstalk](chapter-devenv.md)

**Topics**
+ [Installing Ruby](#ruby-development-environment-ruby)
+ [Installing the AWS SDK for Ruby](#ruby-development-environment-sdk)
+ [Installing an IDE or text editor](#ruby-development-environment-ide)

## Installing Ruby<a name="ruby-development-environment-ruby"></a>

Install GCC if you don't have a C compiler\. On Ubuntu, use `apt`\.

```
~$ sudo apt install gcc
```

On Amazon Linux, use `yum`\.

```
~$ sudo yum install gcc
```

Install RVM to manage Ruby language installations on your machine\. Use the commands at [rvm\.io](https://rvm.io/) to get the project keys and run the installation script\.

```
~$ gpg --keyserver hkp://keys.gnupg.net --recv-keys key1 key2
~$ curl -sSL https://get.rvm.io | bash -s stable
```

This script installs RVM in a folder named `.rvm` in your user directory, and modifies your shell profile to load a setup script whenever you open a new terminal\. Load the script manually to get started\.

```
~$ source ~/.rvm/scripts/rvm
```

Use `rvm get head` to get the latest version\.

```
~$ rvm get head
```

View the available versions of Ruby\.

```
~$ rvm list known
# MRI Rubies
[ruby-]2.1[.10]
[ruby-]2.2[.10]
[ruby-]2.3[.7]
[ruby-]2.4[.4]
[ruby-]2.5[.1]
...
```

Check [Ruby](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.ruby) in the *AWS Elastic Beanstalk Platforms* document to find the latest version of Ruby available on an Elastic Beanstalk platform\. Install that version\.

```
~$ rvm install 2.5.1
Searching for binary rubies, this might take some time.
Found remote file https://rvm_io.global.ssl.fastly.net/binaries/ubuntu/16.04/x86_64/ruby-2.5.1.tar.bz2
Checking requirements for ubuntu.
Requirements installation successful.
ruby-2.5.1 - #configure
ruby-2.5.1 - #download
...
```

Test your Ruby installation\.

```
~$ ruby --version
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux]
```

## Installing the AWS SDK for Ruby<a name="ruby-development-environment-sdk"></a>

If you need to manage AWS resources from within your application, install the AWS SDK for Ruby\. For example, with the SDK for Ruby, you can use Amazon DynamoDB \(DynamoDB\) to store user and session information without creating a relational database\.

Install the SDK for Ruby and its dependencies with the `gem` command\.

```
$ gem install aws-sdk
```

Visit the [AWS SDK for Ruby homepage](https://aws.amazon.com/sdk-for-ruby/) for more information and installation instructions\.

## Installing an IDE or text editor<a name="ruby-development-environment-ide"></a>

Integrated development environments \(IDEs\) provide a wide range of features that facilitate application development\. If you haven't used an IDE for Ruby development, try Aptana and RubyMine and see which works best for you\.
+  [Install Aptana](https://github.com/aptana/studio3) 
+  [RubyMine](https://www.jetbrains.com/ruby/) 

**Note**  
An IDE might add files to your project folder that you might not want to commit to source control\. To prevent committing these files to source control, use `.gitignore` or your source control tool's equivalent\.

If you just want to begin coding and don't need all of the features of an IDE, consider [installing Sublime Text](http://www.sublimetext.com/)\.