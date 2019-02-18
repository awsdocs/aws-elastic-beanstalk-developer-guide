# Serving Static Files<a name="environment-cfg-staticfiles"></a>

To improve performance, you can configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

**To configure the proxy server to serve static files**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. In the **Static files** section, enter a path for serving static files and the directory of the static files to serve into the empty row at the bottom of the list\.

   Start the path with a slash \(`/`\)\. Specify a directory name in the root of your application's source code; don't start it with a slash\.

   When you add a mapping, an extra row appears in case you want to add another one\. To remove a mapping, click the **Remove** icon\.  
![\[Static file configuration in the Modify software configuration page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-static-files.png)

   If you aren't seeing the **Static files** section, you have to add at least one mapping by using a [configuration file](ebextensions.md)\. The specific option namespace to use depends on your environment's platform\. For details, see one of the following pages:
   + [Go Configuration Namespaces](go-environment.md#go-namespaces)
   + [Java SE Configuration Namespaces](java-se-platform.md#java-se-namespaces)
   + [Tomcat Configuration Namespaces](java-tomcat-platform.md#java-tomcat-namespaces)
   + [Node\.js Configuration Namespaces](create_deploy_nodejs.container.md#nodejs-namespaces)
   + [Python Configuration Namespaces](create-deploy-python-container.md#python-namespaces)

1. Choose **Apply**\.