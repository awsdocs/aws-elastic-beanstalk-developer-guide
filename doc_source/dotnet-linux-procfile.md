# Using a Procfile to configure your \.NET Core on Linux environment<a name="dotnet-linux-procfile"></a>

To run multiple applications on the same web server, you must include a `Procfile` in your source bundle that tells Elastic Beanstalk which applications to run\.

We recommend that you always provide a `Procfile` in the source bundle with your application\. This way you precisely control which processes Elastic Beanstalk runs for your application and which arguments these processes receive\.

The following example uses a `Procfile` to specify two applications for Elastic Beanstalk to run on the same web server\.

**Note**  
Procfile contents should be UTF-8 encoded, without a UTF-8 BOM header

The accidental inclusion of a BOM header (likely inserted if using Visual Studio) can lead to `There is no proc command in proc file` deployment errors as the file is not correctly parsed.

**Example Procfile**  

```
web: dotnet ./dotnet-core-app1/dotnetcoreapp1.dll
web2: dotnet ./dotnet-core-app2/dotnetcoreapp2.dll
```

For details about writing and using a `Procfile`, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.
