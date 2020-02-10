# eb platform<a name="eb3-platform"></a>

## Description<a name="eb3-platformdescription"></a>

This command supports two different workspaces:

[Platform](#eb3-platform-preconfigured)  
Use this workspace to manage custom platforms\.

[Environment](#eb3-platform-environment)  
Use this workspace to select a default platform or show information about the current platform\.

Elastic Beanstalk provides the shortcut ebp for eb platform\.

**Note**  
Windows PowerShell uses ebp as a command alias\. If you're running the EB CLI in Windows PowerShell, use the long form of this command — eb platform\.

## Using eb platform for custom platforms<a name="eb3-platform-preconfigured"></a>

Lists the versions of the current platform and enables you to manage custom platforms\.

### Syntax<a name="eb3-platformpresyntax"></a>

eb platform create \[*version*\] \[*options*\]

eb platform delete \[*version*\] \[*options*\]

eb platform events \[*version*\] \[*options*\]

eb platform init \[*platform*\] \[*options*\]

eb platform list \[*options*\]

eb platform logs \[*version*\] \[*options*\]

eb platform status \[*version*\] \[*options*\]

eb platform use \[*platform*\] \[*options*\]

### Options<a name="eb3-platform-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `create [version] [options]`  | Build a new version of the platform\. [Learn more](#eb3-platform-create)\. | 
|  `delete version [options]`  | Delete a platform version\. [Learn more](#eb3-platform-delete)\. | 
|  `events [version] [options]`  | Display the events from a platform version\. [Learn more](#eb3-platform-events)\. | 
|  `init [platform] [options]`  | Initialize a platform repository\. [Learn more](#eb3-platform-init)\. | 
|  `list [options]`  | List the versions of the current platform\. [Learn more](#eb3-platform-list)\. | 
|  `logs [version] [options]`  | Display logs from the builder environment for a platform version\. [Learn more](#eb3-platform-logs)\. | 
|  `status [version] [options]`  | Display the status of the a platform version\. [Learn more](#eb3-platform-status)\. | 
|  `use [platform] [options]`  | Select a different platform from which new versions are built\. [Learn more](#eb3-platform-use)\. | 
|  [Common options](eb3-cmd-options.md)  |  | 

### Common options<a name="eb3-platform-common"></a>

All eb platform commands include the following common options\.


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-h` OR `--help`  |  Shows a help message and exits\.  | 
|  `--debug`  |  Shows additional debugging output\.  | 
|  `--quiet`  |  Suppresses all output\.  | 
|  `-v` OR `--verbose`  |  Shows additional output\.  | 
|  `--profile PROFILE`  |  Uses the specified *PROFILE* from your credentials\.  | 
|  `-r REGION` OR `--region REGION`  |  Use the region *REGION*\.  | 
|  `--no-verify-ssl`  |  Do not verify AWS SSL certificates\.  | 

### Eb platform create<a name="eb3-platform-create"></a>

Builds a new version of the platform and returns the ARN for the new version\. If there is no builder environment running in the current region, this command launches one\. The *version* and increment options \(`-M`, `-m`, and `-p`\) are mutually exclusive\. 

#### Options<a name="eb3-platform-create-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  *version*  |  If *version* isn't specified, creates a new version based on the most\-recent platform with the patch version \(N in n\.n\.N\) incremented\.  | 
|  `-M` OR `--major-increment`  | Increments the major version number \(the N in N\.n\.n\)\. | 
|  `-m` OR `--minor-increment`  | Increments the minor version number \(the N in n\.N\.n\)\. | 
|  `-p` OR `--patch-increment`  | Increments the patch version number \(the N in n\.n\.N\)\. | 
|  `-i INSTANCE_TYPE` OR \-\-instance\-type *INSTANCE\_TYPE*  | Use *INSTANCE\_TYPE* as the instance type, such as **t1\.micro**\. | 
|  `-ip INSTANCE_PROFILE` OR `--instance-profile INSTANCE_PROFILE`  | Use *INSTANCE\_PROFILE* as the instance profile when creating AMIs for a custom platform\. If the `-ip` option isn't specified, creates the instance profile `aws-elasticbeanstalk-custom-platforme-ec2-role` and uses it for the custom platform\. | 
|  `-﻿-﻿tags key1=value1[,key2=value2 ...]`  |  Tag your custom platform version\. Tags are specified as a comma\-separated list of `key=value` pairs\. For more details, see [Tagging custom platform versions](custom-platforms-tagging.md)\.  | 
|  `--timeout` *minutes*  |  Set number of minutes before the command times out\.  | 
|  `--vpc.id VPC_ID`  | The ID of the VPC in which Packer builds\. | 
|  `--vpc.subnets VPC_SUBNETS`  | The VPC subnets in which Packer builds\. | 
|  `--vpc.publicip`  | Associates public IPs to EC2 instances launched\. | 

### Eb platform delete<a name="eb3-platform-delete"></a>

Delete a platform version\. The version isn't deleted if an environment is using that version\.

#### Options<a name="eb3-platform-delete-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `version`  | The version to delete\. This value is required\. | 
|  `--cleanup`  |  Remove all platform versions in the `Failed` state\.  | 
|  `--all-platforms`  |  If `--cleanup` is specified, remove all platform versions in the `Failed` state for all platforms\.  | 
|  `--force`  |  Do not require confirmation when deleting a version\.  | 

### Eb platform events<a name="eb3-platform-events"></a>

Display the events from a platform version\. If *version* is specified, display the events from that version, otherwise display the events from the current version\.

#### Options<a name="eb3-platform-events-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  *version* | The version for which events are displayed\. This value is required\. | 
|  `-f` OR `--follow`  | Continue to display events as they occur\. | 

### Eb platform init<a name="eb3-platform-init"></a>

Initialize a platform repository\.

#### Options<a name="eb3-platform-init-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `platform`  | The name of the platform to initialize\. This value is required, unless `-i` \(interactive mode\) is enabled\. | 
|  `-i` OR `--interactive`  |  Use interactive mode\.  | 
|  `-k KEYNAME` OR `--keyname KEYNAME`  |  The default EC2 key name\.  | 

You can run this command in a directory that has been previously initialized, although you cannot change the workspace type if run in a directory that has been previously initialized\.

To re\-initialize with different options, use the `-i` option\.

### Eb platform list<a name="eb3-platform-list"></a>

List the versions of the platform associated with a workspace \(directory\) or a region\.

The command returns different results depending on the type of workspace you run it in, as follows:
+ In a platform workspace \(a directory initialized by `eb platform init`\), the command returns a list of all platform versions of the custom platform defined in the workspace\. Add the `--all-platforms` or `--verbose` option to get a list of all platform versions of all custom platforms your account has in the region associated with the workspace\.
+ In an application workspace \(a directory initialized by `eb init`\), the command returns a list of all platform versions, both for platforms managed by Elastic Beanstalk and for your account's custom platforms\. The list uses short platform version names, and some platform version variants might be combined\. Add the `--verbose` option to get a detailed list with full names and all variants listed separately\.
+ In an uninitialized directory, the command only works with the `--region` option\. It returns a list of all Elastic Beanstalk\-managed platform versions supported in the region\. The list uses short platform version names, and some platform version variants might be combined\. Add the `--verbose` option to get a detailed list with full names and all variants listed separately\.

#### Options<a name="eb3-platform-list-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-a` OR `--all-platforms`  |  Valid only in an initialized workspace \(a directory initialized by `eb platform init` or `eb init`\)\. Lists the platform versions of all custom platforms associated with your account\. | 
|  `-s STATUS` OR `--status STATUS`  |  List only the platforms matching *STATUS*: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-platform.html)  | 

### Eb platform logs<a name="eb3-platform-logs"></a>

Display logs from the builder environment for a platform version\.

#### Options<a name="eb3-platform-logs-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `version`  |  The version of the platform for which logs are displayed\. If omitted, display logs from the current version\.  | 
|  `--stream`  | Stream deployment logs that were set up with CloudWatch\. | 

### Eb platform status<a name="eb3-platform-status"></a>

Display the status of the a platform version\.

#### Options<a name="eb3-platform-status-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `version`  | The version of the platform for which the status is retrieved\. If omitted, display the status of the current version\. | 

### Eb platform use<a name="eb3-platform-use"></a>

Select a different platform from which new versions are built\.

#### Options<a name="eb3-platform-use-options"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `platform`  | Specifies *platform* as the active version for this workspace\. This value is required\. | 

## Using eb platform for environments<a name="eb3-platform-environment"></a>

Lists supported platforms and enables you to set the default platform and platform version to use when you launch an environment\. Use eb platform list to view a list of all supported platforms\. Use eb platform select to change the platform for your project\. Use eb platform show to view your project's selected platform\.

### Syntax<a name="eb3-platformenvsyntax"></a>

eb platform list

eb platform select

eb platform show

### Options<a name="eb3-platformenvoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `list`  | List the version of the current platform\. | 
|  `select`  | Select the default platform\. | 
|  `show`  | Show information about the current platform\. | 

### Example 1<a name="eb3-platformenvexample1"></a>

The following example lists the names of all configurations for all platforms that Elastic Beanstalk supports\.

```
$ eb platform list
docker-1.5.0
glassfish-4.0-java-7-(preconfigured-docker)
glassfish-4.1-java-8-(preconfigured-docker)
go-1.3-(preconfigured-docker)
go-1.4-(preconfigured-docker)
iis-7.5
iis-8
iis-8.5
multi-container-docker-1.3.3-(generic)
node.js
php-5.3
php-5.4
php-5.5
python
python-2.7
python-3.4
python-3.4-(preconfigured-docker)
ruby-1.9.3
ruby-2.0-(passenger-standalone)
ruby-2.0-(puma)
ruby-2.1-(passenger-standalone)
ruby-2.1-(puma)
ruby-2.2-(passenger-standalone)
ruby-2.2-(puma)
tomcat-6
tomcat-7
tomcat-7-java-6
tomcat-7-java-7
tomcat-8-java-8
```

### Example 2<a name="eb3-platformenvexample2"></a>

The following example prompts you to choose from a list of platforms and the version that you want to deploy for the specified platform\.

```
$ eb platform select
Select a platform.
1) PHP
2) Node.js
3) IIS
4) Tomcat
5) Python
6) Ruby
7) Docker
8) Multi-container Docker
9) GlassFish
10) Go
(default is 1): 5

Select a platform version.
1) Python 2.7
2) Python
3) Python 3.4 (Preconfigured - Docker)
```

### Example 3<a name="eb3-platformenvexample3"></a>

The following example shows information about the current default platform\.

```
$ eb platform show
Current default platform: Python 2.7
New environments will be running:  64bit Amazon Linux 2014.09 v1.2.0 running Python 2.7

Platform info for environment "tmp-dev":
Current: 64bit Amazon Linux 2014.09 v1.2.0 running Python
Latest:  64bit Amazon Linux 2014.09 v1.2.0 running Python
```