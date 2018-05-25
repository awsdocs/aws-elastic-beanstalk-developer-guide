# Platform Specific Options<a name="command-options-specific"></a>

**Topics**
+ [Docker Platform Options](#command-options-docker)
+ [Go Platform Options](#command-options-golang)
+ [Java SE Platform Options](#command-options-plain-java)
+ [Java with Tomcat Platform Options](#command-options-java)
+ [\.NET Platform Options](#command-options-net)
+ [Node\.js Platform Options](#command-options-nodejs)
+ [PHP Platform Options](#command-options-php)
+ [Python Platform Options](#command-options-python)
+ [Ruby Platform Options](#command-options-ruby)

## Docker Platform Options<a name="command-options-docker"></a>

The following Docker\-specific configuration options apply to the Single Container and Preconfigured Docker platforms\.

**Note**  
These configuration options do not apply to the Multicontainer Docker platform\.


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  ProxyServer  |  Specifies the web server to use as a proxy\.  |  `nginx`  |  `nginx` `none`  | 

## Go Platform Options<a name="command-options-golang"></a>

You can use the following namespace to configure the proxy server to serve static files\. When a the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.


**Namespace: `aws:elasticbeanstalk:container:golang:staticfiles`**  

|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

## Java SE Platform Options<a name="command-options-plain-java"></a>

You can use the following namespace to configure the proxy server to serve static files\. When a the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.


**Namespace: `aws:elasticbeanstalk:container:java:staticfiles`**  

|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

Run the AWS X\-Ray daemon to relay trace information from your [X\-Ray integrated](environment-configuration-debugging.md) Java 8 application\.


**Namespace: `aws:elasticbeanstalk:xray`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  `XRayEnabled`  |  Set to `true` to run the AWS X\-Ray daemon on the instances in your environment\.  |  `false`  |  `true` `false`  | 

## Java with Tomcat Platform Options<a name="command-options-java"></a>


**Namespace: `aws:elasticbeanstalk:application:environment`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  JDBC\_CONNECTION\_STRING  |  The connection string to an external database\.  |  n/a  |  n/a  | 

See [Environment Properties and Other Software Settings](environments-cfg-softwaresettings.md) for more information\.


**Namespace: `aws:elasticbeanstalk:container:tomcat:jvmoptions`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  JVM Options  |  Pass command\-line options to the JVM at startup\.  |  n/a  |  n/a  | 
|  Xmx  |  Maximum JVM heap sizes\.  |  `256m`  |  n/a  | 
|  XX:MaxPermSize  |  Section of the JVM heap that is used to store class definitions and associated metadata\.  |  `64m`  |  n/a  | 
|  Xms  |  Initial JVM heap sizes\.  |  `256m`  |  n/a  | 
|  *optionName*  |  Specify arbitrary JVM options in addition to the those defined by the Tomcat platform\.  |  n/a  |  n/a  | 


**Namespace: `aws:elasticbeanstalk:environment:proxy`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  GzipCompression  |  Set to `false` to disable response compression\.  |  `true`  |  `true` `false`  | 
|  ProxyServer  |  Set the proxy to use on your environment's instances\. If you don't set this option, or if you set it to `apache`, Elastic Beanstalk uses [Apache 2\.4](https://httpd.apache.org/docs/2.4/)\. Set to `apache/2.2` if your application isn't ready to migrate away from [Apache 2\.2](https://httpd.apache.org/docs/2.2/) due to incompatible proxy configuration settings\. Set to `nginx` to use [nginx](https://www.nginx.com/)\. For more information, see [Configuring Your Tomcat Environment's Proxy Server](java-tomcat-proxy.md)\.  |  `apache`  |  `apache` `apache/2.2` `nginx`  | 

You can use the following namespace to configure the proxy server to serve static files\. When a the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.


**Namespace: `aws:elasticbeanstalk:environment:proxy:staticfiles`**  

|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

Run the AWS X\-Ray daemon to relay trace information from your [X\-Ray integrated](environment-configuration-debugging.md) Tomcat 8 application\.


**Namespace: `aws:elasticbeanstalk:xray`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  `XRayEnabled`  |  Set to `true` to run the AWS X\-Ray daemon on the instances in your environment\.  |  `false`  |  `true` `false`  | 

## \.NET Platform Options<a name="command-options-net"></a>


**Namespace: `aws:elasticbeanstalk:container:dotnet:apppool`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  Target Runtime  |  Choose the version of \.NET Framework for your application\.  |  `4.0`  |  `2.0` `4.0`  | 
|  Enable 32\-bit Applications  |  Set to `True` to run 32\-bit applications\.  |  `False`  |  `True` `False`  | 

Run the AWS X\-Ray daemon to relay trace information from your [X\-Ray integrated](environment-configuration-debugging.md) \.NET application\.


**Namespace: `aws:elasticbeanstalk:xray`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  `XRayEnabled`  |  Set to `true` to run the AWS X\-Ray daemon on the instances in your environment\.  |  `false`  |  `true` `false`  | 

## Node\.js Platform Options<a name="command-options-nodejs"></a>


**Namespace: `aws:elasticbeanstalk:container:nodejs`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  NodeCommand  |  Command used to start the Node\.js application\. If an empty string is specified, `app.js` is used, then `server.js`, then `npm start` in that order\.  |  ""  |  n/a  | 
|  NodeVersion  |  Version of Node\.js\. For example, `4.4.6` Supported Node\.js versions vary between versions of the Node\.js platform configuration\. See [Node\.js](concepts.platforms.md#concepts.platforms.nodejs) on the supported platforms page for a list of the currently supported versions\.  When support for the version of Node\.js that you are using is removed from the platform configuration, you must change or remove the version setting prior to doing a [platform upgrade](using-features.platform.upgrade.md)\. This may occur when a security vulnerability is identified for one or more versions of Node\.js When this occurs, attempting to upgrade to a new version of the platform that does not support the configured [NodeVersion](#command-options-nodejs) fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a version that is supported by both the old configuration version and the new one, or [remove the option setting](environment-configuration-methods-after.md), and then perform the platform upgrade\.   | varies | varies | 
|  GzipCompression  |  Specifies if gzip compression is enabled\. If ProxyServer is set to `none`, then gzip compression is disabled\.   |  `false`  |  `true` `false`  | 
|  ProxyServer  |  Specifies which web server should be used to proxy connections to Node\.js\. If ProxyServer is set to `none`, then static file mappings doesn't take affect and gzip compression is disabled\.  |  `nginx`  |  `apache` `nginx` `none`  | 

You can use the following namespace to configure the proxy server to serve static files\. When a the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.

**Note**  
Static file settings do not apply if `aws:elasticbeanstalk:container:nodejs::ProxyFiles` is set to `none`\.


**Namespace: `aws:elasticbeanstalk:container:nodejs:staticfiles`**  

|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

Run the AWS X\-Ray daemon to relay trace information from your [X\-Ray integrated](environment-configuration-debugging.md) Node\.js application\.


**Namespace: `aws:elasticbeanstalk:xray`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  `XRayEnabled`  |  Set to `true` to run the AWS X\-Ray daemon on the instances in your environment\.  |  `false`  |  `true` `false`  | 

## PHP Platform Options<a name="command-options-php"></a>


**Namespace: `aws:elasticbeanstalk:container:php:phpini`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  [document\_root](create_deploy_PHP.container.md)  |  Specify the child directory of your project that is treated as the public\-facing web root\.  |  `/`  |  A blank string is treated as `/`, or specify a string starting with `/`  | 
|  [memory\_limit](create_deploy_PHP.container.md)  |  Amount of memory allocated to the PHP environment\.  |  `256M`  |  n/a  | 
|  [zlib\.output\_compression](create_deploy_PHP.container.md)  |  Specifies whether or not PHP should use compression for output\.  |  `Off`  |  `On` `Off`  | 
|  [allow\_url\_fopen](create_deploy_PHP.container.md)  |  Specifies if PHP's file functions are allowed to retrieve data from remote locations, such as websites or FTP servers\.  |  `On`  |  `On` `Off`  | 
|  [display\_errors](create_deploy_PHP.container.md)  |  Specifies if error messages should be part of the output\.  |  `Off`  |  `On` `Off`  | 
|  [max\_execution\_time](create_deploy_PHP.container.md)  |  Sets the maximum time, in seconds, a script is allowed to run before it is terminated by the environment\.  |  `60`  |  `0` to `9223372036854775807` \(PHP\_INT\_MAX\)  | 
|  [composer\_options](create_deploy_PHP.container.md)  |  Sets custom options to use when installing dependencies using Composer through composer\.phar install\. For more information including available options, go to [http://getcomposer\.org/doc/03\-cli\.md\#install](http://getcomposer.org/doc/03-cli.md#install)\.  |  n/a  |  n/a  | 

## Python Platform Options<a name="command-options-python"></a>


**Namespace: `aws:elasticbeanstalk:application:environment`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  DJANGO\_SETTINGS\_MODULE  |  Specifies which settings file to use\.  |  n/a  |  n/a  | 

See [Environment Properties and Other Software Settings](environments-cfg-softwaresettings.md) for more information\.


**Namespace: `aws:elasticbeanstalk:container:python`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  WSGIPath  |  The file that contains the WSGI application\. This file must have an `application` callable\.  |  `application.py`  |  n/a  | 
|  NumProcesses  |  The number of daemon processes that should be started for the process group when running WSGI applications\.  |  `1`  |  n/a  | 
|  NumThreads  |  The number of threads to be created to handle requests in each daemon process within the process group when running WSGI applications\.  |  `15`  |  n/a  | 

You can use the following namespace to configure the proxy server to serve static files\. When a the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. This reduces the number of requests that your application has to process\.

Map a path served by the proxy server to a folder in your source code that contains static assets\. Each option that you define in this namespace maps a different path\.


**Namespace: `aws:elasticbeanstalk:container:python:staticfiles`**  

|  **Name**  |  **Value**  | 
| --- | --- | 
|  Path where the proxy server will serve the files\. Example: `/images` to serve files at `subdomain.eleasticbeanstalk.com/images`\.  |  Name of the folder containing the files\. Example: `staticimages` to serve files from a folder named `staticimages` at the top level of your source bundle\.  | 

Run the AWS X\-Ray daemon to relay trace information from your [X\-Ray integrated](environment-configuration-debugging.md) Python application\.


**Namespace: `aws:elasticbeanstalk:xray`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  `XRayEnabled`  |  Set to `true` to run the AWS X\-Ray daemon on the instances in your environment\.  |  `false`  |  `true` `false`  | 

## Ruby Platform Options<a name="command-options-ruby"></a>


**Namespace: `aws:elasticbeanstalk:application:environment`**  

|  **Name**  |  **Description**  |  **Default**  |  **Valid Values**  | 
| --- | --- | --- | --- | 
|  RAILS\_SKIP\_MIGRATIONS  |  Specifies whether to run ``rake db:migrate`` on behalf of the users' applications; or whether it should be skipped\. This is only applicable to Rails 3 applications\.  |  `false`  |  `true` `false`  | 
|  RAILS\_SKIP\_ASSET\_COMPILATION  |  Specifies whether the container should run ``rake assets:precompile` `on behalf of the users' applications; or whether it should be skipped\. This is also only applicable to Rails 3 applications\.  |  `false`  |  `true` `false`  | 
|  BUNDLE\_WITHOUT  |  A colon \(`:`\) separated list of groups to ignore when installing dependencies from a Gemfile\.  |  `test:development`  |  n/a  | 
|  RACK\_ENV  |  Specifies what environment stage an application can be run in\. Examples of common environments include development, production, test\.  |  `production`  |  n/a  | 

See [Environment Properties and Other Software Settings](environments-cfg-softwaresettings.md) for more information\.