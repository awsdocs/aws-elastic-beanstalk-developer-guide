# Configuring the Reverse Proxy<a name="go-nginx"></a>

Elastic Beanstalk uses nginx as the reverse proxy to map your application to your load balancer on port 80\. If you want to provide your own nginx configuration, you can override the default configuration provided by Elastic Beanstalk by including the `.ebextensions/nginx/nginx.conf` file in your source bundle\. If this file is present, Elastic Beanstalk uses it in place of the default nginx configuration file\.

If you want to include directives in addition to those in the `nginx.conf` `http` block, you can also provide additional configuration files in the `.ebextensions/nginx/conf.d/` directory of your source bundle\. All files in this directory must have the `.conf` extension\. 

To take advantage of functionality provided by Elastic Beanstalk, such as [Enhanced Health Reporting and Monitoring](health-enhanced.md), automatic application mappings, and static files, you must include the following line in the `server` block of your nginx configuration file:

```
include conf.d/elasticbeanstalk/*.conf;
```