# Running multiple applications and ASP\.NET core applications with a deployment manifest<a name="dotnet-manifest"></a>

You can use a deployment manifest to tell Elastic Beanstalk how to deploy your application\. By using this method, you don't need to use `MSDeploy` to generate a source bundle for a single ASP\.NET application that runs at the root path of your website\. Rather, you can use a manifest file to run multiple applications at different paths\. Or, alternatively, you can tell Elastic Beanstalk to deploy and run the app with ASP\.NET Core\. You can also use a deployment manifest to configure an application pool where to run your applications\.

Deployment manifests add support for [\.NET Core applications](#dotnet-manifest-dotnetcore) to Elastic Beanstalk\. You can deploy a \.NET Framework application without a deployment manifest\. However, \.NET Core applications require a deployment manifest to run on Elastic Beanstalk\. When you use a deployment manifest, you create a site archive for each application, and then bundle the site archives in a second ZIP archive that contains the deployment manifest\.

Deployment manifests also add the ability to [run multiple applications at different paths](#dotnet-manifest-multiapp)\. A deployment manifest defines an array of deployment targets, each with a site archive and a path at which IIS should run it\. For example, you could run a web API at the `/api` path to serve asynchronous requests, and a web app at the root path that consumes the API\.

You can also use a deployment manifest to [run multiple applications using application pools in IIS or Kestrel](#dotnet-manifest-apppool)\. You can configure an application pool to restart your applications periodically, run 32\-bit applications, or use a specific version of the \.NET Framework runtime\.

For full customization, you can [write your own deployment scripts](#dotnet-manifest-custom) in Windows PowerShell and tell Elastic Beanstalk which scripts to run to install, uninstall, and restart your application\.

Deployment manifests and related features require a Windows Server platform [version 1\.2\.0 or newer](dotnet-v2migration.md)\.

**Topics**
+ [\.NET core apps](#dotnet-manifest-dotnetcore)
+ [Run multiple applications](#dotnet-manifest-multiapp)
+ [Configure application pools](#dotnet-manifest-apppool)
+ [Define custom deployments](#dotnet-manifest-custom)

## \.NET core apps<a name="dotnet-manifest-dotnetcore"></a>

You can use a deployment manifest to run \.NET Core applications on Elastic Beanstalk\. \.NET Core is a cross\-platform version of \.NET that comes with a command line tool \(`dotnet`\)\. You can use it to generate an application, run it locally, and prepare it for publishing\.

**Note**  
See [Tutorial: Deploying an ASP\.NET core application with Elastic Beanstalk](dotnet-core-tutorial.md) for a tutorial and sample application that use a deployment manifest to run a \.NET Core application on Elastic Beanstalk\.

To run a \.NET Core application on Elastic Beanstalk, you can run `dotnet publish` and package the output in a ZIP archive, not including any containing directories\. Place the site archive in a source bundle with a deployment manifest with a deployment target of type `aspNetCoreWeb`\.

The following deployment manifest runs a \.NET Core application from a site archive named `dotnet-core-app.zip` at the root path\.

**Example aws\-windows\-deployment\-manifest\.json \- \.NET core**  

```
{
  "manifestVersion": 1,
  "deployments": {
    "aspNetCoreWeb": [
      {
        "name": "my-dotnet-core-app",
        "parameters": {
          "archive": "dotnet-core-app.zip",
          "iisPath": "/"
        }
      }
    ]
  }
}
```

Bundle the manifest and site archive in a ZIP archive to create a source bundle\.

**Example dotnet\-core\-bundle\.zip**  

```
.
|-- aws-windows-deployment-manifest.json
`-- dotnet-core-app.zip
```

The site archive contains the compiled application code, dependencies, and `web.config` file\.

**Example dotnet\-core\-app\.zip**  

```
.
|-- Microsoft.AspNetCore.Hosting.Abstractions.dll
|-- Microsoft.AspNetCore.Hosting.Server.Abstractions.dll
|-- Microsoft.AspNetCore.Hosting.dll
|-- Microsoft.AspNetCore.Http.Abstractions.dll
|-- Microsoft.AspNetCore.Http.Extensions.dll
|-- Microsoft.AspNetCore.Http.Features.dll
|-- Microsoft.AspNetCore.Http.dll
|-- Microsoft.AspNetCore.HttpOverrides.dll
|-- Microsoft.AspNetCore.Server.IISIntegration.dll
|-- Microsoft.AspNetCore.Server.Kestrel.dll
|-- Microsoft.AspNetCore.WebUtilities.dll
|-- Microsoft.Extensions.Configuration.Abstractions.dll
|-- Microsoft.Extensions.Configuration.EnvironmentVariables.dll
|-- Microsoft.Extensions.Configuration.dll
|-- Microsoft.Extensions.DependencyInjection.Abstractions.dll
|-- Microsoft.Extensions.DependencyInjection.dll
|-- Microsoft.Extensions.FileProviders.Abstractions.dll
|-- Microsoft.Extensions.FileProviders.Physical.dll
|-- Microsoft.Extensions.FileSystemGlobbing.dll
|-- Microsoft.Extensions.Logging.Abstractions.dll
|-- Microsoft.Extensions.Logging.dll
|-- Microsoft.Extensions.ObjectPool.dll
|-- Microsoft.Extensions.Options.dll
|-- Microsoft.Extensions.PlatformAbstractions.dll
|-- Microsoft.Extensions.Primitives.dll
|-- Microsoft.Net.Http.Headers.dll
|-- System.Diagnostics.Contracts.dll
|-- System.Net.WebSockets.dll
|-- System.Text.Encodings.Web.dll
|-- dotnet-core-app.deps.json
|-- dotnet-core-app.dll
|-- dotnet-core-app.pdb
|-- dotnet-core-app.runtimeconfig.json
`-- web.config
```

See [the tutorial](dotnet-core-tutorial.md) for a full example\.

## Run multiple applications<a name="dotnet-manifest-multiapp"></a>

You can run multiple applications with a deployment manifest by defining multiple deployment targets\.

The following deployment manifest configures two \.NET Core applications\. The `WebAPITest` application implements a few web APIs and serves asynchronous requests at the `/api` path\. The `ASPNetTest` application is a web application that serves requests at the root path\.

**Example aws\-windows\-deployment\-manifest\.json \- multiple apps**  

```
{
  "manifestVersion":  1,
  "deployments": {
    "aspNetCoreWeb": [
      {
        "name": "WebAPITest",
        "parameters": {
          "appBundle": "webapi.zip",
          "iisPath": "/api"
        }
      },
      {
        "name": "ASPNetTest",
        "parameters": {
          "appBundle": "aspnet.zip",
          "iisPath": "/"
        }
      }
    ]
  }
}
```

A sample application with multiple applications is available here:
+ **Deployable source bundle** \- [dotnet\-multiapp\-sample\-bundle\-v2\.zip](samples/dotnet-multiapp-sample-bundle-v2.zip)
+ **Source code** \- [dotnet\-multiapp\-sample\-source\-v2\.zip](samples/dotnet-multiapp-sample-source-v2.zip)

## Configure application pools<a name="dotnet-manifest-apppool"></a>

You can support multiple applications in your Windows environment\. Two approaches are available:
+ You can use the out\-of\-process hosting model with the Kestrel web server\. With this model, you configure multiple applications to run in one application pool\.
+ You can use the in\-process hosting model\.With this model, you use multiple application pools to run multiple applications with only one application in each pool\. If you're using IIS server and need to run multiple applications, you must use this approach\.

To configure Kestrel to run multiple applications in one application pool, add `hostingModel="OutofProcess"` in the `web.config` file\. Consider the following examples\.

**Example web\.config \- for Kestrel out\-of\-process hosting model**  

```
<configuration>
<location path="." inheritInChildApplications="false">
<system.webServer>
<handlers>
<add 
    name="aspNetCore" 
    path="*" verb="*" 
    modules="AspNetCoreModuleV2" 
    resourceType="Unspecified" />
</handlers>
<aspNetCore 
    processPath="dotnet" 
    arguments=".\CoreWebApp-5-0.dll" 
    stdoutLogEnabled="false" 
    stdoutLogFile=".\logs\stdout" 
    hostingModel="OutofProcess" />
</system.webServer>
</location>
</configuration>
```

**Example aws\-windows\-deployment\-manifest\.json \- multiple applications**  

```
{
"manifestVersion": 1,
  "deployments": {"msDeploy": [
      {"name": "Web-app1",
        "parameters": {"archive": "site1.zip",
          "iisPath": "/"
        }
      },
      {"name": "Web-app2",
        "parameters": {"archive": "site2.zip",
          "iisPath": "/app2"
        }
      }
    ]
  }
}
```

IIS doesn't support multiple applications in one application pool because it uses the in\-process hosting model\. Therefore, you need to configure multiple applications by assigning each application to one application pool\. In other words, assign only one application to one application pool\.

You can configure IIS to use different application pools in the `aws-windows-deployment-manifest.json` file\. Make the following updates as you refer to the next example file:
+ Add an `iisConfig` section that includes a subsection called `appPools`\.
+ In the `appPools` block, list the application pools\. 
+ In the `deployments` section, define a `parameters` section for each application\.
+ For each application the `parameters` section specifies an archive, a path to run it, and an `appPool` in which to run\.

The following deployment manifest configures two application pools that restart their application every 10 minutes\. They also attach their applications to a \.NET Framework web application that runs at the path specified\.

**Example aws\-windows\-deployment\-manifest\.json \- one application per application pool**  

```
{
"manifestVersion": 1,
  "iisConfig": {"appPools": [
      {"name": "MyFirstPool",
       "recycling": {"regularTimeInterval": 10}
      },
      {"name": "MySecondPool",
       "recycling": {"regularTimeInterval": 10}
      }
     ]
    },
  "deployments": {"msDeploy": [
      {"name": "Web-app1",
        "parameters": {
           "archive": "site1.zip",
           "iisPath": "/",
           "appPool": "MyFirstPool"
           }
      },
      {"name": "Web-app2",
        "parameters": {
           "archive": "site2.zip",
           "iisPath": "/app2",
           "appPool": "MySecondPool"
          }
      }
     ]
    }
}
```

## Define custom deployments<a name="dotnet-manifest-custom"></a>

For even more control, you can completely customize an application deployment by defining a *custom deployment*\.

The following deployment manifest tells Elastic Beanstalk to run an `install` script named `siteInstall.ps1`\. This script installs the website during instance launch and deployments\. In addition to this, the deployment manifest also tells Elastic Beanstalk to run an `uninstall` script before installing a new version during a deployment and a `restart` script to restart the application when you choose [Restart App Server](environments-console.md#environments-dashboard-actions) in the AWS management console\.

**Example aws\-windows\-deployment\-manifest\.json \- custom deployment**  

```
{
  "manifestVersion": 1,
  "deployments": {
    "custom": [
      {
        "name": "Custom site",
        "scripts": {
          "install": {
            "file": "siteInstall.ps1"
          },
          "restart": {
            "file": "siteRestart.ps1"
          },
          "uninstall": {
            "file": "siteUninstall.ps1"
          }
        }
      }
    ]
  }
}
```

Include any artifacts required to run the application in your source bundle with the manifest and scripts\.

**Example Custom\-site\-bundle\.zip**  

```
.
|-- aws-windows-deployment-manifest.json
|-- siteInstall.ps1
|-- siteRestart.ps1
|-- siteUninstall.ps1
`-- site-contents.zip
```