# Configuring your Tomcat environment's proxy server<a name="java-tomcat-proxy"></a>

The Tomcat platform uses [nginx](https://www.nginx.com/) \(the default\) or [Apache HTTP Server](https://httpd.apache.org/) as the reverse proxy to relay requests from port 80 on the instance to your Tomcat web container listening on port 8080\. Elastic Beanstalk provides a default proxy configuration that you can extend or override completely with your own configuration\.

All Amazon Linux 2 platforms support a uniform proxy configuration feature\. For details about configuring the proxy server on Tomcat platform versions running Amazon Linux 2, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring the proxy on the Amazon Linux AMI \(preceding Amazon Linux 2\) Tomcat platform<a name="java-tomcat-proxy.alami"></a>

If your Elastic Beanstalk Tomcat environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Choosing a proxy server for your Tomcat environment<a name="java-tomcat-proxy.alami"></a>

Tomcat platform versions based on Amazon Linux AMI \(preceding Amazon Linux 2\) use [Apache 2\.4](https://httpd.apache.org/docs/2.4/) for the proxy by default\. You can choose to use [Apache 2\.2](https://httpd.apache.org/docs/2.2/) or [nginx](https://www.nginx.com/) by including a [configuration file](ebextensions.md) in your source code\. The following example configures Elastic Beanstalk to use nginx\.

**Example \.ebextensions/nginx\-proxy\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: nginx
```

### Migrating from Apache 2\.2 to Apache 2\.4<a name="java-tomcat-proxy-apache-migrate"></a>

If your application was developed for [Apache 2\.2](https://httpd.apache.org/docs/2.2/), read this section to learn about migrating to [Apache 2\.4](https://httpd.apache.org/docs/2.4/)\.

Starting with Tomcat platform version 3\.0\.0 configurations, which were released with the [Java with Tomcat platform update on May 24, 2018](https://aws.amazon.com/releasenotes/release-aws-elastic-beanstalk-platform-update-for-the-java-with-tomcat-platform-on-may-24-2018/), Apache 2\.4 is the default proxy of the Tomcat platform\. The Apache 2\.4 `.conf` files are mostly, but not entirely, backward compatible with those of Apache 2\.2\. Elastic Beanstalk includes default `.conf` files that work correctly with each Apache version\. If your application doesn't customize Apache's configuration, as explained in [Extending and overriding the default Apache configuration](#java-tomcat-proxy-apache), it should migrate to Apache 2\.4 without any issues\.

If your application extends or overrides Apache's configuration, you might have to make some changes to migrate to Apache 2\.4\. For more information, see [Upgrading to 2\.4 from 2\.2](https://httpd.apache.org/docs/current/upgrading.html) on *The Apache Software Foundation*'s site\. As a temporary measure, until you successfully migrate to Apache 2\.4, you can choose to use Apache 2\.2 with your application by including the following [configuration file](ebextensions.md) in your source code\.

**Example \.ebextensions/apache\-legacy\-proxy\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: apache/2.2
```

For a quick fix, you can also select the proxy server in the Elastic Beanstalk console\.

**To select the proxy in your Tomcat environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. For **Proxy server**, choose `Apache 2.2 (deprecated)`\.

1. Choose **Apply**\.

![\[Choosing the proxy for a Tomcat environment in the Elastic Beanstalk console's software configuration category\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/java-tomcat-proxy-selection.png)

### Extending and overriding the default Apache configuration<a name="java-tomcat-proxy-apache"></a>

You can extend the Elastic Beanstalk default Apache configuration with your additional configuration files\. Alternatively, you can override the Elastic Beanstalk default Apache configuration completely\.

To extend the Elastic Beanstalk default Apache configuration, add `.conf` configuration files to a folder named `.ebextensions/httpd/conf.d` in your application source bundle\. The Elastic Beanstalk Apache configuration includes `.conf` files in this folder automatically\.

```
~/workspace/my-app/
|-- .ebextensions
|   -- httpd
|      -- conf.d
|         -- myconf.conf
|         -- ssl.conf
-- index.jsp
```

For example, the following Apache 2\.4 configuration adds a listener on port 5000\.

**Example \.ebextensions/httpd/conf\.d/port5000\.conf**  

```
listen 5000
<VirtualHost *:5000>
  <Proxy *>
    Require all granted
  </Proxy>
  ProxyPass / http://localhost:8080/ retry=0
  ProxyPassReverse / http://localhost:8080/
  ProxyPreserveHost on

  ErrorLog /var/log/httpd/elasticbeanstalk-error_log
</VirtualHost>
```

To override the Elastic Beanstalk default Apache configuration completely, include a configuration in your source bundle at `.ebextensions/httpd/conf/httpd.conf`\.

```
~/workspace/my-app/
|-- .ebextensions
|   `-- httpd
|       `-- conf
|           `-- httpd.conf
`-- index.jsp
```

If you override the Elastic Beanstalk Apache configuration, add the following lines to your `httpd.conf` to pull in the Elastic Beanstalk configurations for [Enhanced health reporting and monitoring](health-enhanced.md), response compression, and static files\.

```
IncludeOptional conf.d/*.conf
IncludeOptional conf.d/elasticbeanstalk/*.conf
```

If your environment uses Apache 2\.2 as its proxy, replace the `IncludeOptional` directives with `Include`\. For details about the behavior of these two directives in the two Apache versions, see [Include in Apache 2\.4](https://httpd.apache.org/docs/2.4/mod/core.html#include), [IncludeOptional in Apache 2\.4](https://httpd.apache.org/docs/2.4/mod/core.html#includeoptional), and [Include in Apache 2\.2](https://httpd.apache.org/docs/2.2/mod/core.html#include)\.

**Note**  
To override the default listener on port 80, include a file named `00_application.conf` at `.ebextensions/httpd/conf.d/elasticbeanstalk/` to overwrite the Elastic Beanstalk configuration\.

For a working example, take a look at the Elastic Beanstalk default configuration file at `/etc/httpd/conf/httpd.conf` on an instance in your environment\. All files in the `.ebextensions/httpd` folder in your source bundle are copied to `/etc/httpd` during deployments\.

### Extending the default nginx configuration<a name="java-tomcat-proxy-nginx"></a>

To extend Elastic Beanstalk's default nginx configuration, add `.conf` configuration files to a folder named `.ebextensions/nginx/conf.d/` in your application source bundle\. The Elastic Beanstalk nginx configuration includes `.conf` files in this folder automatically\.

```
~/workspace/my-app/
|-- .ebextensions
|   `-- nginx
|       `-- conf.d
|           |-- elasticbeanstalk
|           |   `-- my-server-conf.conf
|           `-- my-http-conf.conf
`-- index.jsp
```

Files with the \.conf extension in the `conf.d` folder are included in the `http` block of the default configuration\. Files in the `conf.d/elasticbeanstalk` folder are included in the `server` block within the `http` block\.

To override the Elastic Beanstalk default nginx configuration completely, include a configuration in your source bundle at `.ebextensions/nginx/nginx.conf`\.

```
~/workspace/my-app/
|-- .ebextensions
|   `-- nginx
|       `-- nginx.conf
`-- index.jsp
```

If you override the Elastic Beanstalk nginx configuration, add the following line to your configuration's `server` block to pull in the Elastic Beanstalk configurations for the port 80 listener, response compression, and static files\.

```
 include conf.d/elasticbeanstalk/*.conf;
```

**Note**  
To override the default listener on port 80, include a file named `00_application.conf` at `.ebextensions/nginx/conf.d/elasticbeanstalk/` to overwrite the Elastic Beanstalk configuration\.

Also include the following line in your configuration's `http` block to pull in the Elastic Beanstalk configurations for [Enhanced health reporting and monitoring](health-enhanced.md) and logging\.

```
    include       conf.d/*.conf;
```

For a working example, take a look at the Elastic Beanstalk default configuration file at `/etc/nginx/nginx.conf` on an instance in your environment\. All files in the `.ebextensions/nginx` folder in your source bundle are copied to `/etc/nginx` during deployments\.