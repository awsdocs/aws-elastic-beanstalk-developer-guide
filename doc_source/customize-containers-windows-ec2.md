# Customizing software on Windows servers<a name="customize-containers-windows-ec2"></a>

You may want to customize and configure the software that your application depends on\. These files could be either dependencies required by the application—for example, additional packages or services that need to be run\. For general information on customizing and configuring your Elastic Beanstalk environments, see [Configuring Elastic Beanstalk environments](customize-containers.md)\.

**Note**  
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Configuration files support the following keys that affect the Windows server on which your application runs\.

**Topics**
+ [Packages](#windows-packages)
+ [Sources](#windows-sources)
+ [Files](#windows-files)
+ [Commands](#windows-commands)
+ [Services](#windows-services)
+ [Container commands](#windows-container-commands)

Keys are processed in the order that they are listed here\.

**Note**  
Older \(non\-versioned\) \.NET platform versions do not process configuration files in the correct order\. Learn more at [Migrating across major versions of the Elastic Beanstalk Windows server platform](dotnet-v2migration.md)\.

Watch your environment's [events](using-features.events.md) while developing and testing configuration files\. Elastic Beanstalk ignores a configuration file that contains validation errors, like an invalid key, and doesn't process any of the other keys in the same file\. When this happens, Elastic Beanstalk adds a warning event to the event log\.

## Packages<a name="windows-packages"></a>

Use the `packages` key to download and install prepackaged applications and components\.

In Windows environments, Elastic Beanstalk supports downloading and installing MSI packages\. \(Linux environments support additional package managers\. For details, see [Packages](customize-containers-ec2.md#linux-packages) on the *Customizing Software on Linux Servers* page\.\)

You can reference any external location, such as an Amazon Simple Storage Service \(Amazon S3\) object, as long as the URL is publicly accessible\.

If you specify several `msi:` packages, their installation order isn't guaranteed\.

### Syntax<a name="windows-packages-syntax"></a>

Specify a name of your choice as the package name, and a URL to an MSI file location as the value\. You can specify multiple packages under the `msi:` key\.

```
packages: 
  msi:
    package name: package url
    ...
```

### Examples<a name="windows-packages-snippet"></a>

The following example specifies a URL to download **mysql** from `https://dev.mysql.com/`\.

```
packages:
  msi:
    mysql: https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-8.0.11.msi
```

The following example specifies an Amazon S3 object as the MSI file location\.

```
packages:
  msi:
    mymsi: https://mybucket.s3.amazonaws.com/myobject.msi
```

## Sources<a name="windows-sources"></a>

Use the `sources` key to download an archive file from a public URL and unpack it in a target directory on the EC2 instance\.

### Syntax<a name="windows-sources-syntax"></a>

```
sources:  
  target directory: location of archive file
```

### Supported formats<a name="windows-sources-support"></a>

In Windows environments, Elastic Beanstalk supports the \.zip format\. \(Linux environments support additional formats\. For details, see [Sources](customize-containers-ec2.md#linux-sources) on the *Customizing Software on Linux Servers* page\.\)

You can reference any external location, such as an Amazon Simple Storage Service \(Amazon S3\) object, as long as the URL is publicly accessible\.

### Example<a name="windows-sources-example"></a>

The following example downloads a public \.zip file from an Amazon S3 bucket and unpacks it into `c:/myproject/myapp`\.

```
sources:  
  "c:/myproject/myapp": https://mybucket.s3.amazonaws.com/myobject.zip
```

## Files<a name="windows-files"></a>

Use the `files` key to create files on the EC2 instance\. The content can be either inline in the configuration file, or from a URL\. The files are written to disk in lexicographic order\. To download private files from Amazon S3, provide an instance profile for authorization\.

### Syntax<a name="windows-files-syntax"></a>

```
files:  
  "target file location on disk":
    source: URL
    authentication: authentication name:

  "target file location on disk":
    content: |
      this is my content
    encoding: encoding format
```

### Options<a name="windows-files-options"></a>

`content`  
\(Optional\) A string\. 

`source`  
\(Optional\) The URL from which the file is loaded\. This option cannot be specified with the content key\.

`encoding`  
\(Optional\) The encoding format\. This option is only used for a provided content key value\. The default value is `plain`\.  
Valid values: `plain` \| `base64`

`authentication`  
\(Optional\) The name of a [AWS CloudFormation authentication method](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-authentication.html) to use\. You can add authentication methods to the Auto Scaling group metadata with the Resources key\.

### Examples<a name="windows-files-snippet"></a>

The following example shows the two ways to provide file content: from a URL, or inline in the configuration file\.

```
files:
  "c:\\targetdirectory\\targetfile.txt":
    source: http://foo.bar/myfile
 
  "c:/targetdirectory/targetfile.txt":
    content: |
      # this is my file
      # with content
```

**Note**  
If you use a backslash \(\\\) in your file path, you must precede that with another backslash \(the escape character\) as shown in the previous example\.

The following example uses the Resources key to add an authentication method named S3Auth and uses it to download a private file from an Amazon S3 bucket:

```
files:
  "c:\\targetdirectory\\targetfile.zip":
    source: https://elasticbeanstalk-us-east-2-123456789012.s3.amazonaws.com/prefix/myfile.zip
    authentication: S3Auth

Resources:
  AWSEBAutoScalingGroup:
    Metadata:
      AWS::CloudFormation::Authentication:
        S3Auth:
          type: "s3"
          buckets: ["elasticbeanstalk-us-east-2-123456789012"]
          roleName:
            "Fn::GetOptionSetting":
              Namespace: "aws:autoscaling:launchconfiguration"
              OptionName: "IamInstanceProfile"
              DefaultValue: "aws-elasticbeanstalk-ec2-role"
```

## Commands<a name="windows-commands"></a>

Use the `commands` key to execute commands on the EC2 instance\. The commands are processed in alphabetical order by name, and they run before the application and web server are set up and the application version file is extracted\.

The specified commands run as the Administrator user\.

To troubleshoot issues with your commands, you can find their output in [instance logs](using-features.logging.md)\.

### Syntax<a name="windows-commands-syntax"></a>

```
commands:
  command name: 
    command: command to run
```

### Options<a name="windows-commands-options"></a>

`command`  
Either an array or a string specifying the command to run\. If you use an array, you don't need to escape space characters or enclose command parameters in quotation marks\.

`cwd`  
\(Optional\) The working directory\. By default, Elastic Beanstalk attempts to find the directory location of your project\. If not found, it uses `c:\Windows\System32` as the default\.

`env`  
\(Optional\) Sets environment variables for the command\. This property overwrites, rather than appends, the existing environment\.

`ignoreErrors`  
\(Optional\) A Boolean value that determines if other commands should run if the command contained in the `command` key fails \(returns a nonzero value\)\. Set this value to `true` if you want to continue running commands even if the command fails\. Set it to `false` if you want to stop running commands if the command fails\. The default value is `false`\.

`test`  
\(Optional\) A command that must return the value `true` \(exit code 0\) in order for Elastic Beanstalk to process the command contained in the `command` key\.

`waitAfterCompletion`  
\(Optional\) Seconds to wait after the command completes before running the next command\. If the system requires a reboot after the command completes, the system reboots after the specified number of seconds elapses\. If the system reboots as a result of a command, Elastic Beanstalk will recover to the point after the command in the configuration file\. The default value is **60** seconds\. You can also specify **forever**, but the system must reboot before you can run another command\. 

### Example<a name="windows-commands-snippet"></a>

The following example saves the output of the `set` command to the specified file\. If there is a subsequent command, Elastic Beanstalk runs that command immediately after this command completes\. If this command requires a reboot, Elastic Beanstalk reboots the instance immediately after the command completes\.

```
commands:
  test: 
    command: set > c:\\myapp\\set.txt
    waitAfterCompletion: 0
```

## Services<a name="windows-services"></a>

Use the `services` key to define which services should be started or stopped when the instance is launched\. The `services` key also enables you to specify dependencies on sources, packages, and files so that if a restart is needed due to files being installed, Elastic Beanstalk takes care of the service restart\.

### Syntax<a name="windows-services-syntax"></a>

```
services: 
  windows:
    name of service:
      files:
        - "file name"
      sources: 
        - "directory"	
      packages: 
        name of package manager:
            "package name[: version]"
      commands: 
        - "name of command"
```

### Options<a name="windows-services-options"></a>

`ensureRunning`  
\(Optional\) Set to `true` to ensure that the service is running after Elastic Beanstalk finishes\.  
Set to `false` to ensure that the service is not running after Elastic Beanstalk finishes\.  
Omit this key to make no changes to the service state\.

`enabled`  
\(Optional\) Set to `true` to ensure that the service is started automatically upon boot\.  
Set to `false` to ensure that the service is not started automatically upon boot\.  
Omit this key to make no changes to this property\.

`files`  
A list of files\. If Elastic Beanstalk changes one directly via the files block, the service is restarted\.

`sources`  
A list of directories\. If Elastic Beanstalk expands an archive into one of these directories, the service is restarted\.

`packages`  
A map of the package manager to a list of package names\. If Elastic Beanstalk installs or updates one of these packages, the service is restarted\.

`commands`  
A list of command names\. If Elastic Beanstalk runs the specified command, the service is restarted\.

### Example<a name="windows-services-snippet"></a>

```
services: 
  windows:
    myservice:
      enabled: true
      ensureRunning: true
```

## Container commands<a name="windows-container-commands"></a>

Use the `container_commands` key to execute commands that affect your application source code\. Container commands run after the application and web server have been set up and the application version archive has been extracted, but before the application version is deployed\. Non\-container commands and other customization operations are performed prior to the application source code being extracted\.

Container commands are run from the staging directory, where your source code is extracted prior to being deployed to the application server\. Any changes you make to your source code in the staging directory with a container command will be included when the source is deployed to its final location\.

To troubleshoot issues with your container commands, you can find their output in [instance logs](using-features.logging.md)\.

Use the `leader_only` option to only run the command on a single instance, or configure a `test` to only run the command when a test command evaluates to `true`\. Leader\-only container commands are only executed during environment creation and deployments, while other commands and server customization operations are performed every time an instance is provisioned or updated\. Leader\-only container commands are not executed due to launch configuration changes, such as a change in the AMI Id or instance type\.

### Syntax<a name="windows-container-commands-syntax"></a>

```
container_commands:
  name of container_command:
    command: command to run
```

### Options<a name="windows-container-commands-options"></a>

`command`  
A string or array of strings to run\.

`env`  
\(Optional\) Set environment variables prior to running the command, overriding any existing value\.

`cwd`  
\(Optional\) The working directory\. By default, this is the staging directory of the unzipped application\.

`leader_only`  
\(Optional\) Only run the command on a single instance chosen by Elastic Beanstalk\. Leader\-only container commands are run before other container commands\. A command can be leader\-only or have a `test`, but not both \(`leader_only` takes precedence\)\.

`test`  
\(Optional\) Run a test command that must return the `true` in order to run the container command\. A command can be leader\-only or have a `test`, but not both \(`leader_only` takes precedence\)\.

`ignoreErrors`  
\(Optional\) Do not fail deployments if the container command returns a value other than 0 \(success\)\. Set to `true` to enable\.

`waitAfterCompletion`  
\(Optional\) Seconds to wait after the command completes before running the next command\. If the system requires a reboot after the command completes, the system reboots after the specified number of seconds elapses\. If the system reboots as a result of a command, Elastic Beanstalk will recover to the point after the command in the configuration file\. The default value is **60** seconds\. You can also specify **forever**, but the system must reboot before you can run another command\. 

### Example<a name="windows-container-commands-snippet"></a>

The following example saves the output of the `set` command to the specified file\. Elastic Beanstalk runs the command on one instance, and reboots the instance immediately after the command completes\.

```
container_commands:
  foo:
    command: set > c:\\myapp\\set.txt
    leader_only: true
    waitAfterCompletion: 0
```