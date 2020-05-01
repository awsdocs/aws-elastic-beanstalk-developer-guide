# Configuring the reverse proxy<a name="go-nginx"></a>

Elastic Beanstalk uses nginx as the reverse proxy to map your application to your Elastic Load Balancing load balancer on port 80\. Elastic Beanstalk provides a default nginx configuration that you can either extend or override completely with your own configuration\.

By default, Elastic Beanstalk configures the nginx proxy to forward requests to your application on port 5000\. You can override the default port by setting the `PORT` [environment property](go-environment.md#go-options) to the port on which your main application listens\.

**Note**  
The port that your application listens on doesn't affect the port that the nginx server listens to receive requests from the load balancer\.

All Amazon Linux 2 platforms support a uniform proxy configuration feature\. For details about configuring the proxy server on the new Amazon Corretto platform versions running Amazon Linux 2, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring the proxy on Amazon Linux AMI \(preceding Amazon Linux 2\)<a name="go-nginx.alami"></a>

If your Elastic Beanstalk Go environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the information in this section\.

### Extending and overriding the default proxy configuration<a name="go-nginx.alami.extending"></a>

Elastic Beanstalk uses nginx as the reverse proxy to map your application to your load balancer on port 80\. If you want to provide your own nginx configuration, you can override the default configuration provided by Elastic Beanstalk by including the `.ebextensions/nginx/nginx.conf` file in your source bundle\. If this file is present, Elastic Beanstalk uses it in place of the default nginx configuration file\.

If you want to include directives in addition to those in the `nginx.conf` `http` block, you can also provide additional configuration files in the `.ebextensions/nginx/conf.d/` directory of your source bundle\. All files in this directory must have the `.conf` extension\. 

To take advantage of functionality provided by Elastic Beanstalk, such as [Enhanced health reporting and monitoring](health-enhanced.md), automatic application mappings, and static files, you must include the following line in the `server` block of your nginx configuration file:

```
include conf.d/elasticbeanstalk/*.conf;
```