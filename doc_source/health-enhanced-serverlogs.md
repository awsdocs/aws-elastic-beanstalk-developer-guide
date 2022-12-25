# Enhanced health log format<a name="health-enhanced-serverlogs"></a>

AWS Elastic Beanstalk platforms use a custom web server log format to efficiently relay information about HTTP requests to the enhanced health reporting system\. The system analyzes the logs, identifies issues, and sets the instance and environment health accordingly\. If you disable the web server proxy on your environment and serve requests directly from the web container, you can still make full use of enhanced health reporting by configuring your server to output logs in the location and format that the [Elastic Beanstalk health agent](health-enhanced.md#health-enhanced-agent) uses\.

**Note**  
The information on this page is relevant only to Linux\-based platforms\. On the Windows Server platform, Elastic Beanstalk receives information about HTTP requests directly from the IIS web server\. For details, see [Web server metrics capture in IIS on Windows server](health-enhanced-metrics-server-iis.md)\.

## Web server log configuration<a name="health-enhanced-serverlogs.configure"></a>

Elastic Beanstalk platforms are configured to output two logs with information about HTTP requests\. The first is in verbose format and provides detailed information about the request, including the requester's user agent information and a human\-readable timestamp\.

**/var/log/nginx/access\.log**  
The following example is from an nginx proxy running on a Ruby web server environment, but the format is similar for Apache\.

```
172.31.24.3 - - [23/Jul/2015:00:21:20 +0000] "GET / HTTP/1.1" 200 11 "-" "curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3" "177.72.242.17"
172.31.24.3 - - [23/Jul/2015:00:21:21 +0000] "GET / HTTP/1.1" 200 11 "-" "curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3" "177.72.242.17"
172.31.24.3 - - [23/Jul/2015:00:21:22 +0000] "GET / HTTP/1.1" 200 11 "-" "curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3" "177.72.242.17"
172.31.24.3 - - [23/Jul/2015:00:21:22 +0000] "GET / HTTP/1.1" 200 11 "-" "curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3" "177.72.242.17"
172.31.24.3 - - [23/Jul/2015:00:21:22 +0000] "GET / HTTP/1.1" 200 11 "-" "curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3" "177.72.242.17"
```

The second log is in terse format\. It includes information relevant only to enhanced health reporting\. This log is output to a subfolder named `healthd` and rotates hourly\. Old logs are deleted immediately after rotating out\.

**/var/log/nginx/healthd/application\.log\.2015\-07\-23\-00**  
The following example shows a log in the machine\-readable format\.

```
1437609879.311"/"200"0.083"0.083"177.72.242.17
1437609879.874"/"200"0.347"0.347"177.72.242.17
1437609880.006"/bad/path"404"0.001"0.001"177.72.242.17
1437609880.058"/"200"0.530"0.530"177.72.242.17
1437609880.928"/bad/path"404"0.001"0.001"177.72.242.17
```

The enhanced health log format includes the following information:
+ The time of the request, in Unix time
+ The path of the request
+ The HTTP status code for the result
+ The request time
+ The upstream time
+ The `X-Forwarded-For` HTTP header

For nginx proxies, times are printed in floating\-point seconds, with three decimal places\. For Apache, whole microseconds are used\.

**Note**  
If you see a warning similar to the following in a log file, where `DATE-TIME` is a date and time, and you are using a custom proxy, such as in a multi\-container Docker environment, you must use an \.ebextension to configure your environment so that `healthd` can read your log files:  

```
W, [DATE-TIME #1922] WARN -- : log file "/var/log/nginx/healthd/application.log.DATE-TIME" does not exist
```
You can start with the \.ebextension in the [Multicontainer Docker sample](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/docker-multicontainer-v2.zip)\.

**/etc/nginx/conf\.d/webapp\_healthd\.conf**  
The following example shows the log configuration for nginx with the `healthd` log format highlighted\.

```
upstream my_app {
  server unix:///var/run/puma/my_app.sock;
}

log_format healthd '$msec"$uri"'
                '$status"$request_time"$upstream_response_time"'
                '$http_x_forwarded_for';

server {
  listen 80;
  server_name _ localhost; # need to listen to localhost for worker tier

  if ($time_iso8601 ~ "^(\d{4})-(\d{2})-(\d{2})T(\d{2})") {
    set $year $1;
    set $month $2;
    set $day $3;
    set $hour $4;
  }

  access_log  /var/log/nginx/access.log  main;
  access_log /var/log/nginx/healthd/application.log.$year-$month-$day-$hour healthd;

  location / {
    proxy_pass http://my_app; # match the name of upstream directive which is defined above
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

  location /assets {
    alias /var/app/current/public/assets;
    gzip_static on;
    gzip on;
    expires max;
    add_header Cache-Control public;
  }

  location /public {
    alias /var/app/current/public;
    gzip_static on;
    gzip on;
    expires max;
    add_header Cache-Control public;
  }
}
```

**/etc/httpd/conf\.d/healthd\.conf**  
The following example shows the log configuration for Apache\.

```
LogFormat "%{%s}t\"%U\"%s\"%D\"%D\"%{X-Forwarded-For}i" healthd
CustomLog "|/usr/sbin/rotatelogs /var/log/httpd/healthd/application.log.%Y-%m-%d-%H 3600" healthd
```

## Generating logs for enhanced health reporting<a name="health-enhanced-serverlogs.generate"></a>

To provide logs to the health agent, you must do the following:
+ Output logs in the correct format, as shown in the previous section
+ Output logs to `/var/log/nginx/healthd/`
+ Name logs using the following format: `application.log.$year-$month-$day-$hour`
+ Rotate logs once per hour
+ Do not truncate logs