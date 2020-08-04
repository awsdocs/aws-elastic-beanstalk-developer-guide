# Platform specific options<a name="command-options-specific"></a>

Some Elastic Beanstalk platforms define option namespaces that are specific to the platform\. These namespaces and their options are listed below for each platform\.

**Note**  
Previously, in platform versions based on Amazon Linux AMI \(preceding Amazon Linux 2\), the following two features and their respective namespaces were considered to be platform\-specific features, and were listed here per platform:  
**Proxy configuration for static files** – `aws:elasticbeanstalk:environment:proxy:staticfiles`
**AWS X\-Ray support** – `aws:elasticbeanstalk:xray`
In Amazon Linux 2 platform versions, Elastic Beanstalk implements these features in a consistent way across all supporting platforms\. The related namespace are now listed in the [General options for all environments](command-options-general.md) page\. We only kept mention of them on this page for platforms who had differently\-named namespaces\.

**Topics**
+ [Docker platform options](#command-options-docker)
+ [Go platform options](#command-options-golang)
+ [Java SE platform options](#command-options-plain-java)
+ [Java with Tomcat platform options](#command-options-java)
+ [\.NET Core on Linux platform options](#command-options-dotnet-core-linux)
+ [\.NET platform options](#command-options-net)
+ [Node\.js platform options](#command-options-nodejs)
+ [PHP platform options](#command-options-php)
+ [Python platform options](#command-options-python)
+ [Ruby platform options](#command-options-ruby)

## Docker platform options<a name="command-options-docker"></a>

The following Docker\-specific configuration options apply to the Docker and Preconfigured Docker platforms\.

**Note**  
These configuration options do not apply to the Multicontainer Docker platform\.


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  ProxyServer  |  Specifies the web server to use as a proxy\.  |  `nginx`  |  `nginx` `none` – *Amazon Linux AMI only*  | 

## Go platform options<a name="command-options-golang"></a>

### Amazon Linux AMI \(pre\-Amazon Linux 2\) platform options<a name="command-options-golang.alami"></a>

#### Namespace: `aws:elasticbeanstalk:container:golang:staticfiles`<a name="command-options-golang.alami.staticfiles"></a>

You can use the following namespace to configure the proxy server to serve static files\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.


|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

## Java SE platform options<a name="command-options-plain-java"></a>

### Amazon Linux AMI \(pre\-Amazon Linux 2\) platform options<a name="command-options-plain-java.alami"></a>

#### Namespace: `aws:elasticbeanstalk:container:java:staticfiles`<a name="command-options-plain-java.alami.staticfiles"></a>

You can use the following namespace to configure the proxy server to serve static files\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.


|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

## Java with Tomcat platform options<a name="command-options-java"></a>


**Namespace: `aws:elasticbeanstalk:application:environment`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  JDBC\_CONNECTION\_STRING  |  The connection string to an external database\.  |  n/a  |  n/a  | 

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.


**Namespace: `aws:elasticbeanstalk:container:tomcat:jvmoptions`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  JVM Options  |  Pass command\-line options to the JVM at startup\.  |  n/a  |  n/a  | 
|  Xmx  |  Maximum JVM heap sizes\.  |  `256m`  |  n/a  | 
|  XX:MaxPermSize  |  Section of the JVM heap that is used to store class definitions and associated metadata\.  This option only applies to Java versions earlier than Java 8, and isn't supported on Elastic Beanstalk Tomcat platforms based on Amazon Linux 2\.   |  `64m`  |  n/a  | 
|  Xms  |  Initial JVM heap sizes\.  |  `256m`  |  n/a  | 
|  *optionName*  |  Specify arbitrary JVM options in addition to the those defined by the Tomcat platform\.  |  n/a  |  n/a  | 


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  GzipCompression  |  Set to `false` to disable response compression\. *Only valid on Amazon Linux AMI \(preceding Amazon Linux 2\) platform versions\.*  |  `true`  |  `true` `false`  | 
|  ProxyServer  |  Set the proxy to use on your environment's instances\. If you set this option to `apache`, Elastic Beanstalk uses [Apache 2\.4](https://httpd.apache.org/docs/2.4/)\. Set to `apache/2.2` if your application isn't ready to migrate away from [Apache 2\.2](https://httpd.apache.org/docs/2.2/) due to incompatible proxy configuration settings\. *This value is only valid on Amazon Linux AMI \(preceding Amazon Linux 2\) platform versions\.* Set to `nginx` to use [nginx](https://www.nginx.com/)\. This is the default starting with Amazon Linux 2 platform versions\. For more information, see [Configuring your Tomcat environment's proxy server](java-tomcat-proxy.md)\.  |  `nginx` \(Amazon Linux 2\) `apache` \(Amazon Linux AMI\)  |  `apache` `apache/2.2` – *Amazon Linux AMI only* `nginx`  | 

## \.NET Core on Linux platform options<a name="command-options-dotnet-core-linux"></a>


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  ProxyServer  |  Specifies the web server to use as a proxy\.  |  `nginx`  |  `nginx` `none`  | 

## \.NET platform options<a name="command-options-net"></a>


**Namespace: `aws:elasticbeanstalk:container:dotnet:apppool`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  Target Runtime  |  Choose the version of \.NET Framework for your application\.  |  `4.0`  |  `2.0` `4.0`  | 
|  Enable 32\-bit Applications  |  Set to `True` to run 32\-bit applications\.  |  `False`  |  `True` `False`  | 

## Node\.js platform options<a name="command-options-nodejs"></a>


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  ProxyServer  |  Set the proxy to use on your environment's instances\.  |  `nginx`  |  `apache` `nginx`  | 

### Amazon Linux AMI \(pre\-Amazon Linux 2\) platform options<a name="command-options-nodejs.alami"></a>

#### Namespace: `aws:elasticbeanstalk:container:nodejs`<a name="command-options-nodejs.alami.nodejs"></a>


|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  NodeCommand  |  Command used to start the Node\.js application\. If an empty string is specified, `app.js` is used, then `server.js`, then `npm start` in that order\.  |  ""  |  n/a  | 
|  NodeVersion  |  Version of Node\.js\. For example, `4.4.6` Supported Node\.js versions vary between Node\.js platform versions\. See [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* document for a list of the currently supported versions\.  When support for the version of Node\.js that you are using is removed from the platform, you must change or remove the version setting prior to doing a [platform update](using-features.platform.upgrade.md)\. This might occur when a security vulnerability is identified for one or more versions of Node\.js\. When this happens, attempting to update to a new version of the platform that doesn't support the configured [NodeVersion](#command-options-nodejs) fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a Node\.js version that is supported by both the old platform version and the new one, or [remove the option setting](environment-configuration-methods-after.md), and then perform the platform update\.   | varies | varies | 
|  GzipCompression  |  Specifies if gzip compression is enabled\. If ProxyServer is set to `none`, then gzip compression is disabled\.   |  `false`  |  `true` `false`  | 
|  ProxyServer  |  Specifies which web server should be used to proxy connections to Node\.js\. If ProxyServer is set to `none`, then static file mappings doesn't take effect and gzip compression is disabled\.  |  `nginx`  |  `apache` `nginx` `none`  | 

#### Namespace: `aws:elasticbeanstalk:container:nodejs:staticfiles`<a name="command-options-nodejs.alami.staticfiles"></a>

You can use the following namespace to configure the proxy server to serve static files\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.

**Note**  
Static file settings do not apply if `aws:elasticbeanstalk:container:nodejs::ProxyFiles` is set to `none`\.


|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

## PHP platform options<a name="command-options-php"></a>


**Namespace: `aws:elasticbeanstalk:container:php:phpini`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  document\_root  |  Specify the child directory of your project that is treated as the public\-facing web root\.  |  `/`  |  A blank string is treated as `/`, or specify a string starting with `/`  | 
|  memory\_limit  |  Amount of memory allocated to the PHP environment\.  |  `256M`  |  n/a  | 
|  zlib\.output\_compression  |  Specifies whether or not PHP should use compression for output\.  |  `Off`  |  `On` `Off` `true` `false`  | 
|  allow\_url\_fopen  |  Specifies if PHP's file functions are allowed to retrieve data from remote locations, such as websites or FTP servers\.  |  `On`  |  `On` `Off` `true` `false`  | 
|  display\_errors  |  Specifies if error messages should be part of the output\.  |  `Off`  |  `On` `Off`  | 
|  max\_execution\_time  |  Sets the maximum time, in seconds, a script is allowed to run before it is terminated by the environment\.  |  `60`  |  `0` to `9223372036854775807` \(PHP\_INT\_MAX\)  | 
|  composer\_options  |  Sets custom options to use when installing dependencies using Composer through composer\.phar install\. For more information including available options, go to [http://getcomposer\.org/doc/03\-cli\.md\#install](http://getcomposer.org/doc/03-cli.md#install)\.  |  n/a  |  n/a  | 


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  ProxyServer  |  Set the proxy to use on your environment's instances\.  |  `nginx`  |  `apache` `nginx`  | 

**Note**  
For more information about the PHP platform, see [Using the Elastic Beanstalk PHP platform](create_deploy_PHP.container.md)\.

## Python platform options<a name="command-options-python"></a>


**Namespace: `aws:elasticbeanstalk:application:environment`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  DJANGO\_SETTINGS\_MODULE  |  Specifies which settings file to use\.  |  n/a  |  n/a  | 

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.


**Namespace: `aws:elasticbeanstalk:container:python`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  WSGIPath  |  The file that contains the WSGI application\. This file must have an `application` callable\.  |  On Amazon Linux 2 Python platform versions: `application` On Amazon Linux AMI Python platform versions: `application.py`  |  n/a  | 
|  NumProcesses  |  The number of daemon processes that should be started for the process group when running WSGI applications\.  |  `1`  |  n/a  | 
|  NumThreads  |  The number of threads to be created to handle requests in each daemon process within the process group when running WSGI applications\.  |  `15`  |  n/a  | 


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  ProxyServer  |  Set the proxy to use on your environment's instances\.  |  `nginx`  |  `apache` `nginx`  | 

### Amazon Linux AMI \(pre\-Amazon Linux 2\) platform options<a name="command-options-python.alami"></a>

#### Namespace: `aws:elasticbeanstalk:container:python:staticfiles`<a name="command-options-python.alami.staticfiles"></a>

You can use the following namespace to configure the proxy server to serve static files\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.

By default, the proxy server in a Python environment serves any files in a folder named `static` at the `/static` path\.


**Namespace: `aws:elasticbeanstalk:container:python:staticfiles`**  

|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

## Ruby platform options<a name="command-options-ruby"></a>


**Namespace: `aws:elasticbeanstalk:application:environment`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid values**  | 
| --- | --- | --- | --- | 
|  RAILS\_SKIP\_MIGRATIONS  |  Specifies whether to run ``rake db:migrate`` on behalf of the users' applications; or whether it should be skipped\. This is only applicable to Rails 3 applications\.  |  `false`  |  `true` `false`  | 
|  RAILS\_SKIP\_ASSET\_COMPILATION  |  Specifies whether the container should run ``rake assets:precompile` `on behalf of the users' applications; or whether it should be skipped\. This is also only applicable to Rails 3 applications\.  |  `false`  |  `true` `false`  | 
|  BUNDLE\_WITHOUT  |  A colon \(`:`\) separated list of groups to ignore when installing dependencies from a Gemfile\.  |  `test:development`  |  n/a  | 
|  RACK\_ENV  |  Specifies what environment stage an application can be run in\. Examples of common environments include development, production, test\.  |  `production`  |  n/a  | 

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.