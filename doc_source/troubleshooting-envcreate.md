# Environment creation and instance launches<a name="troubleshooting-envcreate"></a>

**Event:** *Failed to Launch Environment*

This event occurs when Elastic Beanstalk attempts to launch an environment and encounters failures along the way\. Previous events on the **Events** page will alert you to the root cause of this issue\.

**Event:** *Create environment operation is complete, but with command timeouts\. Try increasing the timeout period\.*

Your application may take a long time to deploy if you use configuration files that run commands on the instance, download large files, or install packages\. Increase the [command timeout](using-features.rolling-version-deploy.md#environments-cfg-rollingdeployments-console) to give your application more time to start running during deployments\.

**Event:** *The following resource\(s\) failed to create: \[AWSEBInstanceLaunchWaitCondition\]*

This message indicates that your environment's Amazon EC2 instances did not communicate to Elastic Beanstalk that they were launched successfully\. This can occur if the instances do not have Internet connectivity\. If you configured your environment to launch instances in a private VPC subnet, [ensure that the subnet has a NAT](vpc.md) to allow the instances to connect to Elastic Beanstalk\.

**Event:** *A Service Role is required in this region\. Please add a Service Role option to the environment\.*

Elastic Beanstalk uses a service role to monitor the resources in your environment and support [managed platform updates](environment-platform-update-managed.md)\. See [Managing Elastic Beanstalk service roles](iam-servicerole.md) for more information\.