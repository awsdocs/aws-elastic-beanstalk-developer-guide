# Bundling Multiple WAR Files for Tomcat Environments<a name="java-tomcat-multiple-war-files"></a>

If your web app comprises multiple web application components, you can simplify deployments and reduce operating costs by running components in a single environment, instead of running a separate environment for each component\. This strategy is effective for lightweight applications that don't require a lot of resources, and for development and test environments\.

To deploy multiple web applications to your environment, combine each components web application archive \(WAR\) files into a single source bundle\.

To create an application source bundle that contains multiple WAR files, organize the WAR files using the following structure:

```
MyApplication.zip
├── .ebextensions
├── foo.war
├── bar.war
└── ROOT.war
```

When you deploy a source bundle containing multiple WAR files to an AWS Elastic Beanstalk environment, each application is accessible from a different path off of the root domain name\. The preceding example includes three applications: `foo`, `bar`, and `ROOT`\. `ROOT.war` is a special filename that tells Elastic Beanstalk to run that application at the root domain, so that the three applications are available at `http://MyApplication.elasticbeanstalk.com/foo`, `http://MyApplication.elasticbeanstalk.com/bar`, and `http://MyApplication.elasticbeanstalk.com`\.

The source bundle can include only an optional `.ebextensions` folder and WAR files\. The `.ebextensions` folder can contain configuration files that customize the resources deployed to your environment\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **App code**, choose **Upload**\.

1. Choose **Local file**, choose **Browse**, and open the source bundle\.

1. Choose **Upload**\.

1. Choose **Review and launch**\.

1. Review the available settings and choose **Create app**\.

For information about creating source bundles, see [Create an Application Source Bundle](applications-sourcebundle.md)\.