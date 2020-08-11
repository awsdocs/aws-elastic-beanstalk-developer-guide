# Configuring the proxy server for your \.NET Core on Linux environment<a name="dotnet-linux-platform-nginx"></a>

AWS Elastic Beanstalk uses [nginx](https://www.nginx.com/) as the reverse proxy to relay requests to your application\. Elastic Beanstalk provides a default nginx configuration that you can either extend or override completely with your own configuration\.

By default, Elastic Beanstalk configures the nginx proxy to forward requests to your application on port 5000\. You can override the default port by setting the `PORT` [environment property](dotnet-linux-platform.md#dotnet-linux-options-properties) to the port on which your main application listens\.

**Note**  
The port that your application listens on doesn't affect the port that the nginx server listens on receive requests from the load balancer\.

All Amazon Linux 2 platforms support a uniform proxy configuration feature\. For details about configuring the proxy server, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

The following example configuration file extends your environment's nginx configuration\. The configuration directs requests to `/api` to a second web application that listens on port 5200 on the web server\. By default, Elastic Beanstalk forwards requests to a single application that listens on port 5000\.

**Example `01_custom.conf`**  

```
location /api {
     proxy_pass          http://127.0.0.1:5200;
     proxy_http_version  1.1;

     proxy_set_header   Upgrade $http_upgrade;
     proxy_set_header   Connection $http_connection;
     proxy_set_header   Host $host;
     proxy_cache_bypass $http_upgrade;
     proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header   X-Forwarded-Proto $scheme;
}
```