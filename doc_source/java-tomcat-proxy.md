# Configuring Your Tomcat Environment's Proxy Server<a name="java-tomcat-proxy"></a>

The Tomcat platform uses a reverse proxy to relay requests from port 80 on the instance to your Tomcat web container listening on port 8080\. Elastic Beanstalk provides a default proxy configuration that you can either extend or override completely with your own configuration\.

The Tomcat platform uses Apache 2\.2 for the proxy by default\. You can choose to use nginx by including a configuration file in your source code:

**Example \.ebextensions/nginx\-proxy\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: nginx
```


+ [Extending the Default Apache Configuration](#java-tomcat-proxy-apache)
+ [Extending the Default nginx Configuration](#java-tomcat-proxy-nginx)

## Extending the Default Apache Configuration<a name="java-tomcat-proxy-apache"></a>

To extend Elastic Beanstalk's default Apache configuration, add `.conf` configuration files to a folder named `.ebextensions/httpd/conf.d` in your application source bundle\. Elastic Beanstalk's Apache configuration includes `.conf` files in this folder automatically\.

```
~/workspace/my-app/
|-- .ebextensions
|   -- httpd
|      -- conf.d
|         -- myconf.conf
|         -- ssl.conf
-- index.jsp
```

For example, the following configuration adds a listener on port 5000:

**Example \.ebextensions/httpd/conf\.d/port5000\.conf**  

```
listen 5000
<VirtualHost *:5000>
  <Proxy *>
    Order Allow,Deny
    Allow from all
  </Proxy>
  ProxyPass / http://localhost:8080/ retry=0
  ProxyPassReverse / http://localhost:8080/
  ProxyPreserveHost on

  ErrorLog /var/log/httpd/elasticbeanstalk-error_log
</VirtualHost>
```

To override Elastic Beanstalk's default Apache configuration completely, include a configuration in your source bundle at `.ebextensions/httpd/conf/httpd.conf`:

```
~/workspace/my-app/
|-- .ebextensions
|   `-- httpd
|       `-- conf
|           `-- httpd.conf
`-- index.jsp
```

If you override Elastic Beanstalk's Apache configuration, add the following lines to your `httpd.conf` to pull in Elastic Beanstalk's configurations for , response compression, and static files\.

```
Include conf.d/*.conf
Include conf.d/elasticbeanstalk/*.conf
```

**Note**  
To override the default listener on port 80, include a file named `00_application.conf` at `.ebextensions/httpd/conf.d/elasticbeanstalk/` to overwrite Elastic Beanstalk's configuration\.

Take a look at Elastic Beanstalk's default configuration file at `/etc/httpd/conf/httpd.conf` on an instance in your environment for a working example\. All files in the `.ebextensions/httpd` folder in your source bundle are copied to `/etc/httpd` during deployments\.

## Extending the Default nginx Configuration<a name="java-tomcat-proxy-nginx"></a>

To extend Elastic Beanstalk's default nginx configuration, add `.conf` configuration files to a folder named `.ebextensions/nginx/conf.d/` in your application source bundle\. Elastic Beanstalk's nginx configuration includes `.conf` files in this folder automatically\.

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

To override Elastic Beanstalk's default nginx configuration completely, include a configuration in your source bundle at `.ebextensions/nginx/nginx.conf`:

```
~/workspace/my-app/
|-- .ebextensions
|   `-- nginx
|       `-- nginx.conf
`-- index.jsp
```

If you override Elastic Beanstalk's nginx configuration, add the following line to your configuration's `server` block to pull in Elastic Beanstalk's configurations for the port 80 listener, response compression, and static files\.

```
 include conf.d/elasticbeanstalk/*.conf;
```

**Note**  
To override the default listener on port 80, include a file named `00_application.conf` at `.ebextensions/nginx/conf.d/elasticbeanstalk/` to overwrite Elastic Beanstalk's configuration\.

Also include the following line in your configuration's `http` block to pull in Elastic Beanstalk's configurations for  and logging\.

```
    include       conf.d/*.conf;
```

Take a look at Elastic Beanstalk's default configuration file at `/etc/nginx/nginx.conf` on an instance in your environment for a working example\. All files in the `.ebextensions/nginx` folder in your source bundle are copied to `/etc/nginx` during deployments\.