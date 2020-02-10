# Setting up your Go development environment<a name="go-devenv"></a>

Set up a Go development environment to test your application locally before you deploy it to AWS Elastic Beanstalk\. This topic describes the setup steps for your development environment and provides links to installation pages for useful tools\.

For common setup steps and tools that apply to all languages, see [Configuring your development machine for use with Elastic Beanstalk](chapter-devenv.md)\.

## Installing Go<a name="go-devenv-go"></a>

To run Go applications locally, install Go\. If you don't need a specific version, get the latest version that Elastic Beanstalk supports\. For a list of supported versions, see [Go](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.go) in the *AWS Elastic Beanstalk Platforms* document\.

Download Go at [https://golang\.org/doc/install](https://golang.org/doc/install)\.

## Installing the AWS SDK for Go<a name="go-devenv-awssdk"></a>

If you need to manage AWS resources from within your application, install the AWS SDK for Go by using the following command\.

```
$ go get github.com/aws/aws-sdk-go
```

For more information, see [AWS SDK for Go](https://aws.amazon.com/sdk-for-go/)\.