# Using the EB CLI with AWS CodeBuild<a name="eb-cli-codebuild"></a>

You can use the EB CLI to build your application from your source code using AWS CodeBuild\. With AWS CodeBuild, you can compile your source code, run unit tests, and produce artifacts that are ready to deploy\. 

**Note**  
Some regions don't offer AWS CodeBuild\. The integration between Elastic Beanstalk and AWS CodeBuild doesn't work in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## Creating an Application<a name="eb-cli-codebuild-using"></a>

 You can use the Elastic Beanstalk CLI to create an application that uses AWS CodeBuild to compile your source code\. If you run `eb init` in a folder with a `buildspec.yml` file, Elastic Beanstalk detects the file and parses it to detect any Elastic Beanstalk settings\. Elastic Beanstalk extends the [format of the buildspec\.yml file](http://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html) to include the following additional settings, as described in [eb init](eb3-init.md)\. 