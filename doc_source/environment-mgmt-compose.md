# Creating and updating groups of Elastic Beanstalk environments<a name="environment-mgmt-compose"></a>

With the AWS Elastic Beanstalk `Compose Environments` API, you can create and update groups of Elastic Beanstalk environments within a single application\. Each environment in the group can run a separate component of a service\-oriented architecture application\. The `Compose Environments` API takes a list of application versions and an optional group name\. Elastic Beanstalk creates an environment for each application version, or, if the environments already exist, deploys the application versions to them\.

Create links between Elastic Beanstalk environments to designate one environment as a dependency of another\. When you create a group of environments with the `Compose Environments` API, Elastic Beanstalk creates dependent environments only after their dependencies are up and running\. For more information on environment links, see [Creating links between Elastic Beanstalk environments](environment-cfg-links.md)\.

The `Compose Environments` API uses an [environment manifest](environment-cfg-manifest.md) to store configuration details that are shared by groups of environments\. Each component application must have an `env.yaml` configuration file in its application source bundle that specifies the parameters used to create its environment\.

`Compose Environments` requires the `EnvironmentName` and `SolutionStack` to be specified in the environment manifest for each component application\.

You can use the `Compose Environments` API with the Elastic Beanstalk command line interface \(EB CLI\), the AWS CLI, or an SDK\. See [Managing multiple Elastic Beanstalk environments as a group with the EB CLI](ebcli-compose.md) for EB CLI instructions\.

## Using the `Compose Environments` API<a name="environment-mgmt-compose-example"></a>

For example, you could make an application named `Media Library` that lets users upload and manage images and videos stored in Amazon Simple Storage Service \(Amazon S3\)\. The application has a front\-end environment, `front`, that runs a web application that lets users upload and download individual files, view their library, and initiate batch processing jobs\.

Instead of processing the jobs directly, the front\-end application adds jobs to an Amazon SQS queue\. The second environment, `worker`, pulls jobs from the queue and processes them\. `worker` uses a G2 instance type that has a high\-performance GPU, while `front` can run on a more cost\-effective generic instance type\.

You would organize the project folder, `Media Library`, into separate directories for each component, with each directory containing an environment definition file \(`env.yaml`\) with the source code for each:

```
~/workspace/media-library
|-- front
|   `-- env.yaml
`-- worker
    `-- env.yaml
```

The following listings show the `env.yaml` file for each component application\.

**`~/workspace/media-library/front/env.yaml`**

```
EnvironmentName: front+
EnvironmentLinks:
  "WORKERQUEUE" : "worker+"
AWSConfigurationTemplateVersion: 1.1.0.0
EnvironmentTier: 
  Name: WebServer
  Type: Standard
SolutionStack: 64bit Amazon Linux 2015.09 v2.0.4 running Java 8
OptionSettings:
  aws:autoscaling:launchconfiguration:
    InstanceType: m4.large
```

**`~/workspace/media-library/worker/env.yaml`**

```
EnvironmentName: worker+
AWSConfigurationTemplateVersion: 1.1.0.0
EnvironmentTier:
  Name: Worker
  Type: SQS/HTTP
SolutionStack: 64bit Amazon Linux 2015.09 v2.0.4 running Java 8
OptionSettings:
  aws:autoscaling:launchconfiguration:
    InstanceType: g2.2xlarge
```

After [creating an application version](applications-versions.md) for the front\-end \(`front-v1`\) and worker \(`worker-v1`\) application components, you call the `Compose Environments` API with the version names\. In this example, we use the AWS CLI to call the API\.

```
# Create application versions for each component: 
~$ aws elasticbeanstalk create-application-version --application-name media-library --version-label front-v1 --process --source-bundle S3Bucket="my-bucket",S3Key="front-v1.zip"
  {
    "ApplicationVersion": {
        "ApplicationName": "media-library",
        "VersionLabel": "front-v1",
        "Description": "",
        "DateCreated": "2015-11-03T23:01:25.412Z",
        "DateUpdated": "2015-11-03T23:01:25.412Z",
        "SourceBundle": {
            "S3Bucket": "my-bucket",
            "S3Key": "front-v1.zip"
        }
    }
  }
~$ aws elasticbeanstalk create-application-version --application-name media-library --version-label worker-v1 --process --source-bundle S3Bucket="my-bucket",S3Key="worker-v1.zip"
  {
    "ApplicationVersion": {
        "ApplicationName": "media-library",
        "VersionLabel": "worker-v1",
        "Description": "",
        "DateCreated": "2015-11-03T23:01:48.151Z",
        "DateUpdated": "2015-11-03T23:01:48.151Z",
        "SourceBundle": {
            "S3Bucket": "my-bucket",
            "S3Key": "worker-v1.zip"
        }
    }
  }
# Create environments:
~$ aws elasticbeanstalk compose-environments --application-name media-library --group-name dev --version-labels front-v1 worker-v1
```

The third call creates two environments, `front-dev` and `worker-dev`\. The API creates the names of the environments by concatenating the `EnvironmentName` specified in the `env.yaml` file with the `group name` option specified in the `Compose Environments` call, separated by a hyphen\. The total length of these two options and the hyphen must not exceed the maximum allowed environment name length of 23 characters\.

The application running in the `front-dev` environment can access the name of the Amazon SQS queue attached to the `worker-dev` environment by reading the `WORKERQUEUE` variable\. For more information on environment links, see [Creating links between Elastic Beanstalk environments](environment-cfg-links.md)\.