# Configuring the reverse proxy<a name="java-se-nginx"></a>

Elastic Beanstalk uses [nginx](https://www.nginx.com/) as the reverse proxy to map your application to your Elastic Load Balancing load balancer on port 80\. Elastic Beanstalk provides a default nginx configuration that you can either extend or override completely with your own configuration\.

By default, Elastic Beanstalk configures the nginx proxy to forward requests to your application on port 5000\. You can override the default port by setting the `PORT` [environment property](java-se-platform.md#java-se-options) to the port on which your main application listens\.

**Note**  
The port that your application listens on doesn't affect the port that the nginx server listens to receive requests from the load balancer\.

All Amazon Linux 2 platforms support a uniform proxy configuration feature\. For details about configuring the proxy server on the new Amazon Corretto platform versions running Amazon Linux 2, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring the proxy on Amazon Linux AMI \(preceding Amazon Linux 2\)<a name="java-se-nginx.alami"></a>

If your Elastic Beanstalk Java SE environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Extending and overriding the default proxy configuration<a name="java-se-nginx.alami.extending"></a>

To extend Elastic Beanstalk's default nginx configuration, add `.conf` configuration files to a folder named `.ebextensions/nginx/conf.d/` in your application source bundle\. Elastic Beanstalk's nginx configuration includes `.conf` files in this folder automatically\.

```
~/workspace/my-app/
|-- .ebextensions
|   `-- nginx
|       `-- conf.d
|           `-- myconf.conf
`-- web.jar
```

To override Elastic Beanstalk's default nginx configuration completely, include a configuration in your source bundle at `.ebextensions/nginx/nginx.conf`:

```
~/workspace/my-app/
|-- .ebextensions
|   `-- nginx
|       `-- nginx.conf
`-- web.jar
```

If you override Elastic Beanstalk's nginx configuration, add the following line to your `nginx.conf` to pull in Elastic Beanstalk's configurations for [Enhanced health reporting and monitoring](health-enhanced.md), automatic application mappings, and static files\.

```
 include conf.d/elasticbeanstalk/*.conf;
```

The following example configuration from the [Scorekeep sample application](https://github.com/aws-samples/eb-java-scorekeep/) overrides Elastic Beanstalk's default configuration to serve a static web application from the `public` subdirectory of `/var/app/current`, where the Java SE platform copies the application source code\. The `/api` location forwards traffic to routes under `/api/` to the Spring application listening on port 5000\. All other traffic is served by the web app at the root path\.

**Example [\.ebextensions/nginx/nginx\.conf](https://github.com/aws-samples/eb-java-scorekeep/blob/master/.ebextensions/nginx/nginx.conf)**  

```
user                    nginx;
error_log               /var/log/nginx/error.log warn;
pid                     /var/run/nginx.pid;
worker_processes        auto;
worker_rlimit_nofile    33282;

events {
    worker_connections  1024;
}

http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;

  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

  include       conf.d/*.conf;

  map $http_upgrade $connection_upgrade {
      default     "upgrade";
  }

  server {
      listen        80 default_server;
      root /var/app/current/public;

      location / {
      }

      location /api {
          proxy_pass          http://127.0.0.1:5000;
          proxy_http_version  1.1;

          proxy_set_header    Connection          $connection_upgrade;
          proxy_set_header    Upgrade             $http_upgrade;
          proxy_set_header    Host                $host;
          proxy_set_header    X-Real-IP           $remote_addr;
          proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
      }

      access_log    /var/log/nginx/access.log main;

      client_header_timeout 60;
      client_body_timeout   60;
      keepalive_timeout     60;
      gzip                  off;
      gzip_comp_level       4;

      # Include the Elastic Beanstalk generated locations
      include conf.d/elasticbeanstalk/01_static.conf;
      include conf.d/elasticbeanstalk/healthd.conf;
  }
}
```