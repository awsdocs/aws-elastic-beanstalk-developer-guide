# Serving static files<a name="environment-cfg-staticfiles"></a>

To improve performance, you can configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

**To configure the proxy server to serve static files**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and then, in the regions drop\-down list, select your region\.

1. In the navigation pane, choose **Environments**, and then choose your environment's name on the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. In the **Static files** section, enter a path for serving static files and the directory of the static files to serve into the empty row at the bottom of the list\.

   Start the path with a slash \(`/`\)\. Specify a directory name in the root of your application's source code; don't start it with a slash\.

   When you add a mapping, an extra row appears in case you want to add another one\. To remove a mapping, click the **Remove** icon\.  
![\[Static file configuration in the modify software configuration page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-static-files.png)

   If you aren't seeing the **Static files** section, you have to add at least one mapping by using a [configuration file](ebextensions.md)\. The specific option namespace to use depends on your environment's platform\. For details, see one of the following pages:
   + [Go configuration namespaces](go-environment.md#go-namespaces)
   + [Java SE configuration namespaces](java-se-platform.md#java-se-namespaces)
   + [Tomcat configuration namespaces](java-tomcat-platform.md#java-tomcat-namespaces)
   + [Node\.js configuration namespaces](create_deploy_nodejs.container.md#nodejs-namespaces)
   + [Python configuration namespaces](create-deploy-python-container.md#python-namespaces)

1. Choose **Apply**\.