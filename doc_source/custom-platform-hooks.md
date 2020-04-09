# Custom platform hooks<a name="custom-platform-hooks"></a>

Elastic Beanstalk uses a standardized directory structure for hooks on custom platforms\. These are scripts that are run during lifecycle events and in response to management operations: when instances in your environment are launched, or when a user initiates a deployment or uses the restart application server feature\.

Place scripts that you want hooks to trigger in one of the subfolders of the `/opt/elasticbeanstalk/hooks/` folder\.

**Warning**  
*Using custom platform hooks on managed platforms isn't supported\.* Custom platform hooks are designed for custom platforms\. On Elastic Beanstalk managed platforms they might work differently or have some issues, and behavior might differ across platforms\. On Amazon Linux AMI platforms \(preceding Amazon Linux 2\), they might still work in useful ways in some cases; use them with caution\.  
Custom platform hooks are a legacy feature that exists on Amazon Linux AMI platforms\. On Amazon Linux 2 platforms, custom platform hooks in the `/opt/elasticbeanstalk/hooks/` folder are entirely discontinued\. Elastic Beanstalk doesn't read or execute them\. Amazon Linux 2 platforms support a new kind of platform hooks, specifically designed to extend Elastic Beanstalk managed platforms\. You can add custom scripts and programs directly to a hooks directory in your application source bundle\. Elastic Beanstalk runs them during various instance provisioning stages\. For more information, expand the *Platform Hooks* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

Hooks are organized into the following folders:
+ `appdeploy` — Scripts run during an application deployment\. Elastic Beanstalk performs an application deployment when new instances are launched and when a client initiates a new version deployment\.
+ `configdeploy` — Scripts run when a client performs a configuration update that affects the software configuration on instance, for example, by setting environment properties or enabling log rotation to Amazon S3\.
+ `restartappserver` — Scripts run when a client performs a restart app server operation\.
+ `preinit` — Scripts run during instance bootstrapping\.
+ `postinit` — Scripts run after instance bootstrapping\.

The `appdeploy`, `configdeploy`, and `restartappserver` folders contain `pre`, `enact`, and `post` subfolders\. In each phase of an operation, all scripts in the `pre` folder are run in alphabetical order, then those in the `enact` folder, and then those in the `post` folder\.

When an instance is launched, Elastic Beanstalk runs `preinit`, `appdeploy`, and `postinit`, in this order\. On subsequent deployments to running instances, Elastic Beanstalk runs `appdeploy` hooks\. `configdeploy` hooks are run when a user updates instance software configuration settings\. `restartappserver` hooks are run only when the user initiates an application server restart\.

When your scripts encounter errors, they can exit with a non\-zero status and write to `stderr` to fail the operation\. The message that you write to `stderr` will appear in the event that is output when the operation fails\. Elastic Beanstalk also captures this information in the log file `/var/log/eb-activity.log` If you don't want to fail the operation, return 0 \(zero\)\. Messages that you write to `stderr` or `stdout` appear in the [deployment logs](using-features.logging.md), but won't appear in the event stream unless the operation fails\.