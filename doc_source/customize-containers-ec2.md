# Customizing software on Linux servers<a name="customize-containers-ec2"></a>

You may want to customize and configure the software that your application depends on\. You can add commands to be executed during instance provisioning; define Linux users and groups; and download or directly create files on your environment instances\. These files might be either dependencies required by the application—for example, additional packages from the yum repository—or they might be configuration files such as a replacement for a proxy configuration file to override specific settings that are defaulted by Elastic Beanstalk\. 

This section describes the type of information you can include in a configuration file to customize the software on your EC2 instances running Linux\. For general information about customizing and configuring your Elastic Beanstalk environments, see [Configuring Elastic Beanstalk environments](customize-containers.md)\. For information about customizing software on your EC2 instances running Windows, see [Customizing software on Windows servers](customize-containers-windows-ec2.md)\.

**Notes**  
On Amazon Linux 2 platforms, instead of providing files and commands in \.ebextensions configuration files, we highly recommend that you use *Buildfile*\. *Procfile*, and *platform hooks* whenever possible to configure and run custom code on your environment instances during instance provisioning\. For details about these mechanisms, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Configuration files support the following keys that affect the Linux server your application runs on\.

**Topics**
+ [Packages](#linux-packages)
+ [Groups](#linux-groups)
+ [Users](#linux-users)
+ [Sources](#linux-sources)
+ [Files](#linux-files)
+ [Commands](#linux-commands)
+ [Services](#linux-services)
+ [Container commands](#linux-container-commands)
+ [Example: Using custom amazon CloudWatch metrics](customize-containers-cw.md)

Keys are processed in the order that they are listed here\.

Watch your environment's [events](using-features.events.md) while developing and testing configuration files\. Elastic Beanstalk ignores a configuration file that contains validation errors, like an invalid key, and doesn't process any of the other keys in the same file\. When this happens, Elastic Beanstalk adds a warning event to the event log\.

## Packages<a name="linux-packages"></a>

You can use the `packages` key to download and install prepackaged applications and components\.

### Syntax<a name="linux-packages-syntax"></a>

```
packages: 
  name of package manager:
    package name: version
    ...
  name of package manager:
    package name: version
    ...
  ...
```

You can specify multiple packages under each package manager's key\.

### Supported package formats<a name="linux-packages-support"></a>

Elastic Beanstalk currently supports the following package managers: yum, rubygems, python, and rpm\. Packages are processed in the following order: rpm, yum, and then rubygems and python\. There is no ordering between rubygems and python\. Within each package manager, package installation order isn't guaranteed\. Use a package manager supported by your operating system\.

**Note**  
Elastic Beanstalk supports two underlying package managers for Python, pip and easy\_install\. However, in the syntax of the configuration file, you must specify the package manager name as `python`\. When you use a configuration file to specify a Python package manager, Elastic Beanstalk uses Python 2\.7\. If your application relies on a different version of Python, you can specify the packages to install in a `requirements.txt` file\. For more information, see [Specifying dependencies using a requirements file](python-configuration-requirements.md)\.

### Specifying versions<a name="linux-packages-versions"></a>

Within each package manager, each package is specified as a package name and a list of versions\. The version can be a string, a list of versions, or an empty string or list\. An empty string or list indicates that you want the latest version\. For rpm manager, the version is specified as a path to a file on disk or a URL\. Relative paths are not supported\.

If you specify a version of a package, Elastic Beanstalk attempts to install that version even if a newer version of the package is already installed on the instance\. If a newer version is already installed, the deployment fails\. Some package managers support multiple versions, but others may not\. Please check the documentation for your package manager for more information\. If you do not specify a version and a version of the package is already installed, Elastic Beanstalk does not install a new version—it assumes that you want to keep and use the existing version\.

### Example snippet<a name="linux-packages-snippet"></a>

The following snippet specifies a version URL for rpm, requests the latest version from yum, and version 0\.10\.2 of chef from rubygems\.

```
packages: 
  yum:
    libmemcached: [] 
    ruby-devel: []
    gcc: []
  rpm:
    epel: http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm
  rubygems: 
    chef: '0.10.2'
```

## Groups<a name="linux-groups"></a>

You can use the `groups` key to create Linux/UNIX groups and to assign group IDs\. To create a group, add a new key\-value pair that maps a new group name to an optional group ID\. The groups key can contain one or more group names\. The following table lists the available keys\.

### Syntax<a name="linux-groups-syntax"></a>

```
groups:
  name of group: {}
  name of group:
    gid: "group id"
```

### Options<a name="linux-groups-options"></a>

`gid`  
A group ID number\.  
If a group ID is specified, and the group already exists by name, the group creation will fail\. If another group has the specified group ID, the operating system may reject the group creation\.

### Example snippet<a name="linux-groups-snippet"></a>

The following snippet specifies a group named groupOne without assigning a group ID and a group named groupTwo that specified a group ID value of 45\.

```
groups:
  groupOne: {}
  groupTwo:
    gid: "45"
```

## Users<a name="linux-users"></a>

You can use the `users` key to create Linux/UNIX users on the EC2 instance\.

### Syntax<a name="linux-users-syntax"></a>

```
users:
  name of user:
    groups:
      - name of group
    uid: "id of the user"
    homeDir: "user's home directory"
```

### Options<a name="linux-users-options"></a>

`uid`  
A user ID\. The creation process fails if the user name exists with a different user ID\. If the user ID is already assigned to an existing user, the operating system may reject the creation request\.

`groups`  
A list of group names\. The user is added to each group in the list\.

`homeDir`  
The user's home directory\.

Users are created as noninteractive system users with a shell of `/sbin/nologin`\. This is by design and cannot be modified\.

### Example snippet<a name="linux-users-snippet"></a>

```
users:
  myuser:
    groups:
      - group1
      - group2
    uid: "50"
    homeDir: "/tmp"
```

## Sources<a name="linux-sources"></a>

You can use the `sources` key to download an archive file from a public URL and unpack it in a target directory on the EC2 instance\.

### Syntax<a name="linux-sources-syntax"></a>

```
sources:
  target directory: location of archive file
```

### Supported formats<a name="linux-sources-support"></a>

Supported formats are tar, tar\+gzip, tar\+bz2, and zip\. You can reference external locations such as Amazon Simple Storage Service \(Amazon S3\) \(e\.g\., `https://mybucket.s3.amazonaws.com/myobject`\) as long as the URL is publicly accessible\.

### Example snippet<a name="linux-sources-example"></a>

The following example downloads a public \.zip file from an Amazon S3 bucket and unpacks it into `/etc/myapp`:

```
sources:  
  /etc/myapp: https://mybucket.s3.amazonaws.com/myobject
```

**Note**  
Multiple extractions should not reuse the same target path\. Extracting another source to the same target path will replace rather than append to the contents\. 

## Files<a name="linux-files"></a>

You can use the `files` key to create files on the EC2 instance\. The content can be either inline in the configuration file, or the content can be pulled from a URL\. The files are written to disk in lexicographic order\.

You can use the `files` key to download private files from Amazon S3 by providing an instance profile for authorization\.

If the file path you specify already exists on the instance, the existing file is retained with the extension `.bak` appended to its name\.

### Syntax<a name="linux-files-syntax"></a>

```
files:  
  "target file location on disk": 
     mode: "six-digit octal value"
     owner: name of owning user for file
     group: name of owning group for file
     source: URL
     authentication: authentication name:

  "target file location on disk": 
     mode: "six-digit octal value"
     owner: name of owning user for file
     group: name of owning group for file
     content: |
      # this is my
      # file content
     encoding: encoding format
     authentication: authentication name:
```

### Options<a name="linux-files-options"></a>

`content`  
String content to add to the file\. Specify either `content` or `source`, but not both\.

`source`  
URL of a file to download\. Specify either `content` or `source`, but not both\.

`encoding`  
The encoding format of the string specified with the `content` option\.  
Valid values: `plain` \| `base64`

`group`  
Linux group that owns the file\.

`owner`  
Linux user that owns the file\.

`mode`  
A six\-digit octal value representing the mode for this file\. Not supported for Windows systems\. Use the first three digits for symlinks and the last three digits for setting permissions\. To create a symlink, specify `120xxx`, where `xxx` defines the permissions of the target file\. To specify permissions for a file, use the last three digits, such as `000644`\.

`authentication`  
The name of a [AWS CloudFormation authentication method](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-authentication.html) to use\. You can add authentication methods to the Auto Scaling group metadata with the Resources key\. See below for an example\.

### Example snippet<a name="linux-files-snippet"></a>

```
files:
  "/home/ec2-user/myfile" :
    mode: "000755"
    owner: root
    group: root
    source: http://foo.bar/myfile
 
  "/home/ec2-user/myfile2" :
    mode: "000755"
    owner: root
    group: root
    content: |
      this is my
      file content
```

Example using a symlink\. This creates a link `/tmp/myfile2.txt` that points at the existing file `/tmp/myfile1.txt`\.

```
files:
  "/tmp/myfile2.txt" :
    mode: "120400"
    content: "/tmp/myfile1.txt"
```

The following example uses the Resources key to add an authentication method named S3Auth and uses it to download a private file from an Amazon S3 bucket:

```
Resources:
  AWSEBAutoScalingGroup:
    Metadata:
      AWS::CloudFormation::Authentication:
        S3Auth:
          type: "s3"
          buckets: ["elasticbeanstalk-us-west-2-123456789012"]
          roleName:
            "Fn::GetOptionSetting":
              Namespace: "aws:autoscaling:launchconfiguration"
              OptionName: "IamInstanceProfile"
              DefaultValue: "aws-elasticbeanstalk-ec2-role"

files:
  "/tmp/data.json" :
    mode: "000755"
    owner: root
    group: root
    authentication: "S3Auth"
    source: https://elasticbeanstalk-us-west-2-123456789012.s3-us-west-2.amazonaws.com/data.json
```

## Commands<a name="linux-commands"></a>

You can use the `commands` key to execute commands on the EC2 instance\. The commands run before the application and web server are set up and the application version file is extracted\.

The specified commands run as the root user, and are processed in alphabetical order by name\. By default, commands run in the root directory\. To run commands from another directory, use the `cwd` option\.

To troubleshoot issues with your commands, you can find their output in [instance logs](using-features.logging.md)\.

### Syntax<a name="linux-commands-syntax"></a>

```
commands:
  command name: 
    command: command to run
    cwd: working directory
    env: 
      variable name: variable value
    test: conditions for command 
    ignoreErrors: true
```

### Options<a name="linux-commands-options"></a>

`command`  
Either an array \([block sequence collection](http://yaml.org/spec/1.2/spec.html#id2759963) in YAML syntax\) or a string specifying the command to run\. Some important notes:  
+ If you use a string, you don't need to enclose the entire string in quotes\. If you do use quotes, escape literal occurrences of the same type of quote\.
+ If you use an array, you don't need to escape space characters or enclose command parameters in quotes\. Each array element is a single command argument\. Don't use an array to specify multiple commands\.
The following examples are all equivalent:  

```
commands:
  command1:
    command: git commit -m "This is a comment."
  command2:
    command: "git commit -m \"This is a comment.\""
  command3:
    command: 'git commit -m "This is a comment."'
  command4:
    command:
      - git
      - commit
      - -m
      - This is a comment.
```
To specify multiple commands, use a [literal block scalar](http://yaml.org/spec/1.2/spec.html#id2760844), as shown in the following example\.  

```
commands:
  command block:
    command: |
      git commit -m "This is a comment."
      git push
```

`env`  
\(Optional\) Sets environment variables for the command\. This property overwrites, rather than appends, the existing environment\.

`cwd`  
\(Optional\) The working directory\. If not specified, commands run from the root directory \(/\)\.

`test`  
\(Optional\) A command that must return the value `true` \(exit code 0\) in order for Elastic Beanstalk to process the command, such as a shell script, contained in the `command` key\.

`ignoreErrors`  
\(Optional\) A boolean value that determines if other commands should run if the command contained in the `command` key fails \(returns a nonzero value\)\. Set this value to `true` if you want to continue running commands even if the command fails\. Set it to `false` if you want to stop running commands if the command fails\. The default value is `false`\.

### Example snippet<a name="linux-commands-snippet"></a>

The following example snippet runs a Python script\.

```
commands:
  python_install:
    command: myscript.py
    cwd: /home/ec2-user
    env:
      myvarname: myvarvalue
    test: "[ -x /usr/bin/python ]"
```

## Services<a name="linux-services"></a>

You can use the `services` key to define which services should be started or stopped when the instance is launched\. The `services` key also allows you to specify dependencies on sources, packages, and files so that if a restart is needed due to files being installed, Elastic Beanstalk takes care of the service restart\.

### Syntax<a name="linux-services-syntax"></a>

```
services:
  sysvinit:
    name of service:
      enabled: "true"
      ensureRunning: "true"
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

### Options<a name="linux-services-options"></a>

`ensureRunning`  
Set to `true` to ensure that the service is running after Elastic Beanstalk finishes\.  
Set to `false` to ensure that the service is not running after Elastic Beanstalk finishes\.  
Omit this key to make no changes to the service state\.

`enabled`  
Set to `true` to ensure that the service is started automatically upon boot\.  
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

### Example snippet<a name="linux-services-snippet"></a>

The following is an example snippet:

```
services: 
  sysvinit:
    myservice:
      enabled: true
      ensureRunning: true
```

## Container commands<a name="linux-container-commands"></a>

You can use the `container_commands` key to execute commands that affect your application source code\. Container commands run after the application and web server have been set up and the application version archive has been extracted, but before the application version is deployed\. Non\-container commands and other customization operations are performed prior to the application source code being extracted\.

The specified commands run as the root user, and are processed in alphabetical order by name\. Container commands are run from the staging directory, where your source code is extracted prior to being deployed to the application server\. Any changes you make to your source code in the staging directory with a container command will be included when the source is deployed to its final location\.

To troubleshoot issues with your container commands, you can find their output in [instance logs](using-features.logging.md)\.

You can use `leader_only` to only run the command on a single instance, or configure a `test` to only run the command when a test command evaluates to `true`\. Leader\-only container commands are only executed during environment creation and deployments, while other commands and server customization operations are performed every time an instance is provisioned or updated\. Leader\-only container commands are not executed due to launch configuration changes, such as a change in the AMI Id or instance type\.

### Syntax<a name="linux-container-commands-syntax"></a>

```
container_commands:
  name of container_command:
    command: "command to run"
    leader_only: true
  name of container_command:
    command: "command to run"
```

### Options<a name="linux-container-commands-options"></a>

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

### Example snippet<a name="linux-container-commands-snippet"></a>

The following is an example snippet\.

```
container_commands:
  collectstatic:
    command: "django-admin.py collectstatic --noinput"
  01syncdb:
    command: "django-admin.py syncdb --noinput"
    leader_only: true
  02migrate:
    command: "django-admin.py migrate"
    leader_only: true
  99customize:
    command: "scripts/customize.sh"
```