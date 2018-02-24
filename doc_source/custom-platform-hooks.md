# Platform Hooks<a name="custom-platform-hooks"></a>

Elastic Beanstalk uses a standardized directory structure for hooks, which are scripts that are run during lifecycle events and in response to management operations: when instances in your environment are launched, or when a user initiates a deployment or uses the restart application server feature\.

Hooks are organized into the following folders:

+ `appdeploy` — Scripts run during an application deployment\. Elastic Beanstalk performs an application deployment when new instances are launched and when a client initiates a new version deployment\.

+ `configdeploy` — Scripts run when a client performs a configuration update that affects the software configuration on\-instance, for example, by setting environment properties or enabling log rotation to Amazon S3\.

+ `restartappserver` — Scripts run when a client performs a restart app server operation\.

+ `preinit` — Scripts run during instance bootstrapping\.

+ `postinit` — Scripts run after instance bootstrapping\.

The `appdeploy`, `configdeploy`, and `restartappserver` folders contain `pre`, `enact`, and `post` subfolders\. In each phase of an operation, all scripts in the `pre` folder are run in alphabetical order, then the `enact` folder, then the `post` folder\.

When an instance is launched, Elastic Beanstalk runs `preinit`, `appdeploy`, and `postinit`, in this order\. On subsequent deployments to running instances, Elastic Beanstalk runs `appdeploy` hooks\. `configdeploy` hooks are run when a user updates instance software configuration settings\. `restartappserver` hooks are run only when the user initiates an application server restart\.

When your scripts encounter errors, they can exit with a non\-zero status and write to `stderr` to fail the operation\. The message that you write to `stderr` will appear in the event that is output when the operation fails\. Elastic Beanstalk also captures this information in the log file `/var/log/eb-activity.log` If you don't want to fail the operation, return 0\. Messages that you write to stderr or stdout appear in the [deployment logs](using-features.logging.md), but won't appear in the event stream unless the operation fails\.