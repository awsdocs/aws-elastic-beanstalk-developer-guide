# Deployments<a name="troubleshooting-deployments"></a>

**Issue:** *Application becomes unavailable during deployments*

Because Elastic Beanstalk uses a drop\-in upgrade process, there might be a few seconds of downtime\. Use [rolling deployments](using-features.rolling-version-deploy.md) to minimize the effect of deployments on your production environments\.

**Event:** *Failed to create the AWS Elastic Beanstalk application version*

Your application source bundle may be too large, or you may have reached the [application version quota](applications-versions.md)\.

**Event:** *Update environment operation is complete, but with command timeouts\. Try increasing the timeout period\.*

Your application may take a long time to deploy if you use configuration files that run commands on the instance, download large files, or install packages\. Increase the [command timeout](using-features.rolling-version-deploy.md#environments-cfg-rollingdeployments-console) to give your application more time to start running during deployments\.