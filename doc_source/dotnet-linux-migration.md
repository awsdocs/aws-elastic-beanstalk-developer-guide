# Migrating from \.NET on Windows Server platforms to the \.NET Core on Linux platform<a name="dotnet-linux-migration"></a>

You can migrate applications that run on [\.NET on Windows Server](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net) platforms to the \.NET Core on Linux platforms\. Following are some considerations when migrating from Windows to Linux platforms\.

## Considerations for migrating to the \.NET Core on Linux platform<a name="dotnet-linux-migration.considerations"></a>


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Application configuration  |  On Windows platforms, you use a [deployment manifest](dotnet-manifest.md) to specify the applications that run in your environment\. The \.NET Core on Linux platforms use a [Procfile](dotnet-linux-procfile.md) to specify the applications that run on your environment's instances\. For details on bundling applications, see [Bundling applications for the \.NET Core on Linux platform](dotnet-linux-platform-bundle-app.md)\.  | 
|  Proxy server  |  On Windows platforms, you use IIS as your application's proxy server\. The \.NET Core on Linux platforms include nginx as a reverse proxy by default\. You can choose to use no proxy server and use Kestrel as your application's web server\. To learn more, see [Configuring the proxy server for your \.NET Core on Linux environment](dotnet-linux-platform-nginx.md)\.  | 
|  Routing  |  On Windows platforms, you use IIS in your application code and include a [deployment manifest](dotnet-manifest.md) to configure the IIS path\. For the \.NET Core on Linux platform, you use [ASP \.NET Core routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing?view=aspnetcore-3.1) in your application code, and update your environment's nginx configuration\. To learn more, see [Configuring the proxy server for your \.NET Core on Linux environment](dotnet-linux-platform-nginx.md)\.  | 
|  Logs  |  The Linux and Windows platforms stream different logs\. For details, see [How Elastic Beanstalk sets up CloudWatch Logs](AWSHowTo.cloudwatchlogs.md#AWSHowTo.cloudwatchlogs.loggroups)\.  | 