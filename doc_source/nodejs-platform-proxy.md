# Configuring the proxy server<a name="nodejs-platform-proxy"></a>

Elastic Beanstalk uses nginx or Apache HTTPD as the reverse proxy to map your application to your Elastic Load Balancing load balancer on port 80\. The default is nginx\. Elastic Beanstalk provides a default proxy configuration that you can either extend or override completely with your own configuration\.

By default, Elastic Beanstalk configures the proxy to forward requests to your application on port 8080\. You can override the default port by setting the `PORT` [environment property](create_deploy_nodejs.container.md#nodejs-platform-console) to the port on which your main application listens\.

**Notes**  
The port that your application listens on doesn't affect the port that the nginx server listens to receive requests from the load balancer\.
On Amazon Linux AMI Node\.js platform versions \(preceding Amazon Linux 2\), Elastic Beanstalk configures the proxy to forward requests to your application on port 8081\. For details about proxy configuration on these Node\.js platform versions, see [Configuring the proxy on Amazon Linux AMI \(preceding Amazon Linux 2\)](#nodejs-platform-proxy.alami) on this page\.

All Amazon Linux 2 platforms support a uniform proxy configuration feature\. For details about configuring the proxy server on the new Amazon Corretto platform versions running Amazon Linux 2, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring the proxy on Amazon Linux AMI \(preceding Amazon Linux 2\)<a name="nodejs-platform-proxy.alami"></a>

If your Elastic Beanstalk Node\.js environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the information in this section\.

### Extending and overriding the default proxy configuration<a name="nodejs-platform-proxy.alami.extending"></a>

The Node\.js platform uses a reverse proxy to relay requests from port 80 on the instance to your application listening on port 8081\. Elastic Beanstalk provides a default proxy configuration that you can either extend or override completely with your own configuration\.

To extend the default configuration, add `.conf` files to `/etc/nginx/conf.d` with a configuration file\. See [Terminating HTTPS on EC2 instances running Node\.js](https-singleinstance-nodejs.md) for an example\.

The Node\.js platform sets the PORT environment variable to the port to which the proxy server passes traffic\. Read this variable in your code to configure your application's port\.

```
    var port = process.env.PORT || 3000;

    var server = app.listen(port, function () {
        console.log('Server running at http://127.0.0.1:' + port + '/');
    });
```

The default nginx configuration forwards traffic to an upstream server named `nodejs` at `127.0.0.1:8081`\. It is possible to remove the default configuration and provide your own in a [configuration file](ebextensions.md)\.

**Example \.ebextensions/proxy\.config**  
The following example removes the default configuration and adds a custom configuration that forwards traffic to port 5000 instead of 8081\.  

```
files:
  /etc/nginx/conf.d/proxy.conf:
    mode: "000644"
    owner: root
    group: root
    content: |
      upstream nodejs {
        server 127.0.0.1:5000;
        keepalive 256;
      }

      server {
        listen 8080;

        if ($time_iso8601 ~ "^(\d{4})-(\d{2})-(\d{2})T(\d{2})") {
            set $year $1;
            set $month $2;
            set $day $3;
            set $hour $4;
        }
        access_log /var/log/nginx/healthd/application.log.$year-$month-$day-$hour healthd;
        access_log  /var/log/nginx/access.log  main;

        location / {
            proxy_pass  http://nodejs;
            proxy_set_header   Connection "";
            proxy_http_version 1.1;
            proxy_set_header        Host            $host;
            proxy_set_header        X-Real-IP       $remote_addr;
            proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        gzip on;
        gzip_comp_level 4;
        gzip_types text/html text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

        location /static {
            alias /var/app/current/static;
        }

      }

  /opt/elasticbeanstalk/hooks/configdeploy/post/99_kill_default_nginx.sh:
    mode: "000755"
    owner: root
    group: root
    content: |
      #!/bin/bash -xe
      rm -f /etc/nginx/conf.d/00_elastic_beanstalk_proxy.conf
      service nginx stop 
      service nginx start

container_commands:
  removeconfig:
    command: "rm -f /tmp/deployment/config/#etc#nginx#conf.d#00_elastic_beanstalk_proxy.conf /etc/nginx/conf.d/00_elastic_beanstalk_proxy.conf"
```
The example configuration, `/etc/nginx/conf.d/proxy.conf`, uses the default configuration at `/etc/nginx/conf.d/00_elastic_beanstalk_proxy.conf` as a base to include the default server block with compression and log settings, and a static file mapping\.  
The `removeconfig` command removes the container's default configuration to make sure that the proxy server uses the custom configuration\. Elastic Beanstalk recreates the default configuration during every configuration deployment\. To account for that, the example adds a post\-configuration\-deployment hook, `/opt/elasticbeanstalk/hooks/configdeploy/post/99_kill_default_nginx.sh`, which removes the default configuration and restarts the proxy server\.

**Note**  
The default configuration may change in future versions of the Node\.js platform\. Use the latest version of the configuration as a base for your customizations to ensure compatibility\.

If you override the default configuration, you must define any static file mappings and gzip compression, as the platform will not be able to apply the [standard settings](create_deploy_nodejs.container.md#nodejs-namespaces)\.