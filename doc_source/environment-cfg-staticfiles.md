# Serving static files<a name="environment-cfg-staticfiles"></a>

To improve performance, you can configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

Elastic Beanstalk supports configuring the proxy to serve static files on most platform branches based on Amazon Linux 2\. The one exception is Docker\.

**Note**  
On the Python and Ruby platforms, Elastic Beanstalk configures some static file folders by default\. For details, see the static file configuration sections for [Python](create-deploy-python-container.md#python-platform-staticfiles) and [Ruby](create_deploy_Ruby.container.md#create_deploy_Ruby.container.console.staticfiles)\. You can configure additional folders as explained on this page\.

## Configure static files using the console<a name="environment-cfg-staticfiles.console"></a>

**To configure the proxy server to serve static files**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. In the **Static files** section, enter a path for serving static files and the directory of the static files to serve into the empty row at the bottom of the list\.
**Note**  
If you aren't seeing the **Static files** section, you have to add at least one mapping by using a [configuration file](ebextensions.md)\. For details, see [Configure static files using configuration options](#environment-cfg-staticfiles.namespace) on this page\.

   Start the path with a slash \(`/`\)\. Specify a directory name in the root of your application's source code; don't start it with a slash\.

   When you add a mapping, an extra row appears in case you want to add another one\. To remove a mapping, click the **Remove** icon\.  
![\[Static file configuration in the modify software configuration page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-static-files.png)

1. Choose **Apply**\.

## Configure static files using configuration options<a name="environment-cfg-staticfiles.namespace"></a>

You can use a [configuration file](ebextensions.md) to configure static file paths and directory locations using configuration options\. You can add a configuration file to your application's source bundle and deploy it during environment creation or a later deployment\.

If your environment uses a platform branch based on Amazon Linux 2, use the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace\.

The following example configuration file tells the proxy server to serve files in the `statichtml` folder at the path `/html`, and files in the `staticimages` folder at the path `/images`\.

**Example \.ebextensions/static\-files\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /html: statichtml
    /images: staticimages
```

If your Elastic Beanstalk environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the following additional information:

### Amazon Linux AMI platform\-specific namespaces<a name="environment-cfg-staticfiles.namespace.specific"></a>

On Amazon Linux AMI platform branches, static file configuration namespaces vary by platform\. For details, see one of the following pages:
+ [Go configuration namespace](go-environment.md#go-namespaces)
+ [Java SE configuration namespace](java-se-platform.md#java-se-namespaces)
+ [Tomcat configuration namespaces](java-tomcat-platform.md#java-tomcat-namespaces)
+ [Node\.js configuration namespace](create_deploy_nodejs.container.md#nodejs-namespaces)
+ [Python configuration namespaces](create-deploy-python-container.md#python-namespaces)