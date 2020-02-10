# Creating links between Elastic Beanstalk environments<a name="environment-cfg-links"></a>

As your application grows in size and complexity, you may want to split it into components that have different development and operational lifecycles\. By running smaller services that interact with each other over a well defined interface, teams can work independently and deployments can be lower risk\. AWS Elastic Beanstalk lets you link your environments to share information between components that depend on one another\.

**Note**  
Elastic Beanstalk currently supports environment links for all platforms except Multicontainer Docker\.

With environment links, you can specify the connections between your application’s component environments as named references\. When you create an environment that defines a link, Elastic Beanstalk sets an environment variable with the same name as the link\. The value of the variable is the endpoint that you can use to connect to the other component, which can be a web server or worker environment\.

For example, if your application consists of a frontend that collects email addresses and a worker that sends a welcome email to the email addresses collected by the frontend, you can create a link to the worker in your frontend and have the frontend automatically discover the endpoint \(queue URL\) for your worker\.

Define links to other environments in an [environment manifest](environment-cfg-manifest.md), a YAML formatted file named `env.yaml` in the root of your application source\. The following manifest defines a link to an environment named worker:

**`~/workspace/my-app/frontend/env.yaml`**

```
AWSConfigurationTemplateVersion: 1.1.0.0
EnvironmentLinks:
  "WORKERQUEUE": "worker"
```

When you create an environment with an application version that includes the above environment manifest, Elastic Beanstalk looks for an environment named `worker` that belongs to the same application\. If that environment exists, Elastic Beanstalk creates an environment property named `WORKERQUEUE`\. The value of `WORKERQUEUE` is the Amazon SQS queue URL\. The frontend application can read this property in the same manner as an environment variable\. See [Environment manifest \(`env.yaml`\)](environment-cfg-manifest.md) for details\.

To use environment links, add an environment manifest to your application source and upload it with the EB CLI, AWS CLI or an SDK\. If you use the AWS CLI or an SDK, set the `process` flag when you call `CreateApplicationVersion`: 

```
$ aws elasticbeanstalk create-application-version --process --application-name my-app --version-label frontend-v1 --source-bundle S3Bucket="my-bucket",S3Key="front-v1.zip"
```

This option tells Elastic Beanstalk to validate the environment manifest and configuration files in your source bundle when you create the application version\. The EB CLI sets this flag automatically when you have an environment manifest in your project directory\.

Create your environments normally using any client\. When you need to terminate environments, terminate the environment with the link first\. If an environment is linked to by another environment, Elastic Beanstalk will prevent the linked environment from being terminated\. To override this protection, use the `ForceTerminate` flag\. This parameter is available in the AWS CLI as `--force-terminate`:

```
$ aws elasticbeanstalk terminate-environment --force-terminate --environment-name worker
```