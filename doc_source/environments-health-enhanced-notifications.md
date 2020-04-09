# Notifications and troubleshooting<a name="environments-health-enhanced-notifications"></a>

This page lists example cause messages for common issues and links to more information\. Cause messages appear in the [environment overview](environment-health-console.md) page of the Elastic Beanstalk console and are recorded in [events](using-features.events.md) when health issues persist across several checks\.

## Deployments<a name="environments-health-enhanced-notifications-deployments"></a>

Elastic Beanstalk monitors your environment for consistency following deployments\. If a rolling deployment fails, the version of your application running on the instances in your environment may vary\. This can occur if a deployment succeeds on one or more batches but fails prior to all batches completing\.

*Incorrect application version found on 2 out of 5 instances\. Expected version "v1" \(deployment 1\)\.*

*Incorrect application version on environment instances\. Expected version "v1" \(deployment 1\)\.*

The expected application version is not running on some or all instances in an environment\.

*Incorrect application version "v2" \(deployment 2\)\. Expected version "v1" \(deployment 1\)\.*

The application deployed to an instance differs from the expected version\. If a deployment fails, the expected version is reset to the version from the most recent successful deployment\. In the above example, the first deployment \(version "v1"\) succeeded, but the second deployment \(version "v2"\) failed\. Any instances running "v2" are considered unhealthy\.

To solve this issue, start another deployment\. You can [redeploy a previous version](using-features.deploy-existing-version.md) that you know works, or configure your environment to [ignore health checks](using-features.rolling-version-deploy.md#environments-cfg-rollingdeployments-console) during deployment and redeploy the new version to force the deployment to complete\.

You can also identify and terminate the instances that are running the wrong application version\. Elastic Beanstalk will launch instances with the correct version to replace any instances that you terminate\. Use the [EB CLI health command](health-enhanced-ebcli.md) to identify instances that are running the wrong application version\.

## Application server<a name="environments-health-enhanced-notifications-webapp"></a>

*15% of requests are erroring with HTTP 4xx*

*20% of the requests to the ELB are erroring with HTTP 4xx\.*

A high percentage of HTTP requests to an instance or environment are failing with 4xx errors\.

A 400 series status code indicates that the user made a bad request, such as requesting a page that doesn't exist \(404 File Not Found\) or that the user doesn't have access to \(403 Forbidden\)\. A low number of 404s is not unusual but a large number could mean that there are internal or external links to unavailable pages\. These issues can be resolved by fixing bad internal links and adding redirects for bad external links\.

*5% of the requests are failing with HTTP 5xx*

*3% of the requests to the ELB are failing with HTTP 5xx\.*

A high percentage of HTTP requests to an instance or environment are failing with 500 series status codes\.

A 500 series status code indicates that the application server encountered an internal error\. These issues indicate that there is an error in your application code and should be identified and fixed quickly\.

*95% of CPU is in use*

On an instance, the health agent is reporting an extremely high percentage of CPU usage and sets the instance health to **Warning** or **Degraded**\.

Scale your environment to take load off of instances\.

## Worker instance<a name="environments-health-enhanced-notifications-worker"></a>

*20 messages waiting in the queue \(25 seconds ago\)*

Requests are being added to your worker environment's queue faster than they can be processed\. Scale your environment to increase capacity\.

*5 messages in Dead Letter Queue \(15 seconds ago\)*

Worker requests are failing repeatedly and being added to the [Dead\-letter queues](using-features-managing-env-tiers.md#worker-deadletter)\. Check the requests in the dead\-letter queue to see why they are failing\. 

## Other resources<a name="environments-health-enhanced-notifications-other"></a>

*4 active instances is below Auto Scaling group minimum size 5*

The number of instances running in your environment is fewer than the minimum configured for the Auto Scaling group\.

*Auto Scaling group \(groupname\) notifications have been deleted or modified*

The notifications configured for your Auto Scaling group have been modified outside of Elastic Beanstalk\.