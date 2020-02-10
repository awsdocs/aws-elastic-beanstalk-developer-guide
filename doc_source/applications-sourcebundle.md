# Create an application source bundle<a name="applications-sourcebundle"></a>

When you use the AWS Elastic Beanstalk console to deploy a new application or an application version, you'll need to upload a source bundle\. Your source bundle must meet the following requirements: 
+ Consist of a single `ZIP` file or `WAR` file \(you can include multiple `WAR` files inside your `ZIP` file\)
+ Not exceed 512 MB
+ Not include a parent folder or top\-level directory \(subdirectories are fine\)

If you want to deploy a worker application that processes periodic background tasks, your application source bundle must also include a `cron.yaml` file\. For more information, see [Periodic tasks](using-features-managing-env-tiers.md#worker-periodictasks)\.

If you are deploying your application with the Elastic Beanstalk Command Line Interface \(EB CLI\), the AWS Toolkit for Eclipse, or the AWS Toolkit for Visual Studio, the ZIP or WAR file will automatically be structured correctly\. For more information, see [Using the Elastic Beanstalk command line interface \(EB CLI\)](eb-cli3.md), [Creating and deploying Java applications on Elastic Beanstalk](create_deploy_Java.md), and [The AWS Toolkit for Visual Studio](dotnet-toolkit.md)\.

**Topics**
+ [Creating a source bundle from the command line](#using-features.deployment.source.commandline)
+ [Creating a source bundle with Git](#using-features.deployment.source.git)
+ [Zipping files in Mac OS X Finder or Windows explorer](#using-features.deployment.source.gui)
+ [Creating a source bundle for a \.NET application](#using-features.deployment.source.dotnet)
+ [Testing your source bundle](#using-features.deployment.source.test)

## Creating a source bundle from the command line<a name="using-features.deployment.source.commandline"></a>

Create a source bundle using the `zip` command\. To include hidden files and folders, use a pattern like the following\.

```
~/myapp$ zip ../myapp.zip -r * .[^.]*
  adding: app.js (deflated 63%)
  adding: index.js (deflated 44%)
  adding: manual.js (deflated 64%)
  adding: package.json (deflated 40%)
  adding: restify.js (deflated 85%)
  adding: .ebextensions/ (stored 0%)
  adding: .ebextensions/xray.config (stored 0%)
```

This ensures that Elastic Beanstalk [configuration files](ebextensions.md) and other files and folders that start with a period are included in the archive\.

For Tomcat web applications, use `jar` to create a web archive\.

```
~/myapp$ jar -cvf myapp.war .
```

The above commands include hidden files that may increase your source bundle size unnecessarily\. For more control, use a more detailed file pattern, or [create your source bundle with Git](#using-features.deployment.source.git)\.

## Creating a source bundle with Git<a name="using-features.deployment.source.git"></a>

If you're using Git to manage your application source code, use the `git archive` command to create your source bundle\.

```
$ git archive -v -o myapp.zip --format=zip HEAD
```

`git archive` only includes files that are stored in git, and excludes ignored files and git files\. This helps keep your source bundle as small as possible\. For more information, go to the [git\-archive manual page](http://git-scm.com/docs/git-archive)\.

## Zipping files in Mac OS X Finder or Windows explorer<a name="using-features.deployment.source.gui"></a>

When you create a `ZIP` file in Mac OS X Finder or Windows Explorer, make sure you zip the files and subfolders themselves, rather than zipping the parent folder\. 

**Note**  
The graphical user interface \(GUI\) on Mac OS X and Linux\-based operating systems does not display files and folders with names that begin with a period \(\.\)\. Use the command line instead of the GUI to compress your application if the `ZIP` file must include a hidden folder, such as `.ebextensions`\. For command line procedures to create a `ZIP` file on Mac OS X or a Linux\-based operating system, see [Creating a source bundle from the command line](#using-features.deployment.source.commandline)\.

**Example**  
Suppose you have a Python project folder labeled `myapp`, which includes the following files and subfolders:   

```
myapplication.py
README.md
static/
static/css
static/css/styles.css
static/img
static/img/favicon.ico
static/img/logo.png
templates/
templates/base.html
templates/index.html
```
As noted in the list of requirements above, your source bundle must be compressed without a parent folder, so that its decompressed structure does not include an extra top\-level directory\. In this example, no `myapp` folder should be created when the files are decompressed \(or, at the command line, no `myapp` segment should be added to the file paths\)\.   
This sample file structure is used throughout this topic to illustrate how to zip files\.

**To zip files in Mac OS X Finder**

1. Open your top\-level project folder and select all the files and subfolders within it\. Do not select the top\-level folder itself\.  
![\[Files selected in Mac OS X Finder\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/finder-files.png)

1. Right\-click the selected files, and then choose **Compress** *X* **items**, where *X* is the number of files and subfolders you've selected\.  
![\[Compressing files in Mac OS X Finder\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/finder-context.png)

**To zip files in Windows explorer**

1. Open your top\-level project folder and select all the files and subfolders within it\. Do not select the top\-level folder itself\.  
![\[Files selected in Windows explorer\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/winex-files.png)

1. Right\-click the selected files, choose **Send to**, and then choose **Compressed \(zipped\) folder**\.  
![\[Compressing files in Windows explorer\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/winex-context.png)

## Creating a source bundle for a \.NET application<a name="using-features.deployment.source.dotnet"></a>

If you use Visual Studio, you can use the deployment tool included in the AWS Toolkit for Visual Studio to deploy your \.NET application to Elastic Beanstalk\. For more information, see [Deploying Elastic Beanstalk applications in \.NET using the deployment tool](deploy_NET_standalone_tool.md)\.

If you need to manually create a source bundle for your \.NET application, you cannot simply create a `ZIP` file that contains the project directory\. You must create a web deployment package for your project that is suitable for deployment to Elastic Beanstalk\. There are several methods you can use to create a deployment package:
+ Create the deployment package using the **Publish Web** wizard in Visual Studio\. For more information, go to [How to: Create a Web Deployment Package in Visual Studio](http://msdn.microsoft.com/en-us/library/dd465323.aspx)\.
**Important**  
When creating the web deployment package, you must start the **Site name** with `Default Web Site`\.
+ It you have a \.NET project, you can create the deployment package using the msbuild command as shown in the following example\. 
**Important**  
The `DeployIisAppPath` parameter must begin with `Default Web Site`\.

  ```
  C:/> msbuild <web_app>.csproj /t:Package /p:DeployIisAppPath="Default Web Site"
  ```
+ If you have a website project, you can use the IIS Web Deploy tool to create the deployment package\. For more information, go to [Packaging and Restoring a Web site](http://www.iis.net/learn/publish/using-web-deploy/packaging-and-restoring-a-web-site)\.
**Important**  
The `apphostconfig` parameter must begin with `Default Web Site`\.

If you are deploying multiple applications or an ASP\.NET Core application, put your `.ebextensions` folder in the root of the source bundle, side by side with the application bundles and manifest file:

```
~/workspace/source-bundle/
|-- .ebextensions
|   |-- environmentvariables.config
|   `-- healthcheckurl.config
|-- AspNetCore101HelloWorld.zip
|-- AspNetCoreHelloWorld.zip
|-- aws-windows-deployment-manifest.json
`-- VS2015AspNetWebApiApp.zip
```

## Testing your source bundle<a name="using-features.deployment.source.test"></a>

You may want to test your source bundle locally before you upload it to Elastic Beanstalk\. Because Elastic Beanstalk essentially uses the command line to extract the files, it's best to do your tests from the command line rather than with a GUI tool\. 

**To test the file extraction in Mac OS X or Linux**

1. Open a terminal window \(Mac OS X\) or connect to the Linux server\. Navigate to the directory that contains your source bundle\.

1. Using the `unzip` or `tar xf` command, decompress the archive\.

1. Ensure that the decompressed files appear in the same folder as the archive itself, rather than in a new top\-level folder or directory\.
**Note**  
If you use Mac OS X Finder to decompress the archive, a new top\-level folder will be created, no matter how you structured the archive itself\. For best results, use the command line\.

**To test the file extraction in Windows**

1. Download or install a program that allows you to extract compressed files via the command line\. For example, you can download the free unzip\.exe program from [http://stahlforce\.com/dev/index\.php?tool=zipunzip](http://stahlforce.com/dev/index.php?tool=zipunzip)\.

1. If necessary, copy the executable file to the directory that contains your source bundle\. If you've installed a system\-wide tool, you can skip this step\.

1. Using the appropriate command, decompress the archive\. If you downloaded unzip\.exe using the link in step 1, the command is `unzip <archive-name>`\.

1. Ensure that the decompressed files appear in the same folder as the archive itself, rather than in a new top\-level folder or directory\.