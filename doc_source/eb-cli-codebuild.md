# Using the EB CLI with AWS CodeBuild<a name="eb-cli-codebuild"></a>

[AWS CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/) compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. You can use CodeBuild together with the EB CLI to automate building your application from its source code\. Environment creation and each deployment thereafter start with a build step, and then deploy the resulting application\.

**Note**  
Some regions don't offer CodeBuild\. The integration between Elastic Beanstalk and CodeBuild doesn't work in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## Creating an application<a name="eb-cli-codebuild-using"></a>

**To create an Elastic Beanstalk application that uses CodeBuild**

1. Include a CodeBuild build specification file, [https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html), in your application folder\.

1. Add an `eb_codebuild_settings` entry with options specific to Elastic Beanstalk to the file\.

1. Run [eb init](eb3-init.md) in the folder\.

Elastic Beanstalk extends the [CodeBuild build specification file format](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html) to include the following additional settings:

```
eb_codebuild_settings:
  CodeBuildServiceRole: role-name
  ComputeType: size
  Image: image
  Timeout: minutes
```

`CodeBuildServiceRole`  
The ARN or name of the AWS Identity and Access Management \(IAM\) service role that CodeBuild can use to interact with dependent AWS services on your behalf\. This value is required\. If you omit it, any subsequent eb create or eb deploy command fails\.  
To learn more about creating a service role for CodeBuild, see [Create a CodeBuild Service Role](https://docs.aws.amazon.com/codebuild/latest/userguide/setting-up.html#setting-up-service-role) in the *AWS CodeBuild User Guide*\.  
You also need permissions to perform actions in CodeBuild itself\. The Elastic Beanstalk `AWSElasticBeanstalkFullAccess` managed user policy includes all the required CodeBuild action permissions\. If you're not using the managed policy, be sure to allow the following permissions in your user policy\.  

```
  "codebuild:CreateProject",
  "codebuild:DeleteProject",
  "codebuild:BatchGetBuilds",
  "codebuild:StartBuild"
```
For details, see [Managing Elastic Beanstalk user policies](AWSHowTo.iam.managed-policies.md)\.

`ComputeType`  
The amount of resources used by the Docker container in the CodeBuild build environment\. Valid values are BUILD\_GENERAL1\_SMALL, BUILD\_GENERAL1\_MEDIUM, and BUILD\_GENERAL1\_LARGE\.

`Image`  
The name of the Docker Hub or Amazon ECR image that CodeBuild uses for the build environment\. This Docker image should contain all the tools and runtime libraries required to build your code, and should match your application's target platform\. CodeBuild manages and maintains a set of images specifically meant to be used with Elastic Beanstalk\. It is recommended that you use one of them\. For details, see [Docker Images Provided by CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-available.html) in the *AWS CodeBuild User Guide*\.  
The `Image` value is optional\. If you omit it, the eb init command attempts to choose an image that best matches your target platform\. In addition, if you run eb init in interactive mode and it fails to choose an image for you, it prompts you to choose one\. At the end of a successful initialization, eb init writes the chosen image into the `buildspec.yml` file\.

`Timeout`  
The duration, in minutes, that the CodeBuild build runs before timing out\. This value is optional\. For details about valid and default values, see [Create a Build Project in CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/create-project.html)\.  
This timeout controls the maximum duration for a CodeBuild run, and the EB CLI also respects it as part of its first step to create an application version\. It's distinct from the value you can specify with the `--timeout` option of the [eb create](eb3-create.md) or [eb deploy](eb3-deploy.md) commands\. The latter value controls the maximum duration that for EB CLI to wait for environment creation or update\.

## Building and deploying your application code<a name="eb-cli-codebuild-using"></a>

Whenever your application code needs to be deployed, the EB CLI uses CodeBuild to run a build, then deploys the resulting build artifacts to your environment\. This happens when you create an Elastic Beanstalk environment for your application using the [eb create](eb3-create.md) command, and each time you later deploy code changes to the environment using the [eb deploy](eb3-deploy.md) command\.

If the CodeBuild step fails, environment creation or deployment doesn't start\.