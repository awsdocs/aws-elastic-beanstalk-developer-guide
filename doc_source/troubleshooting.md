# Troubleshooting<a name="troubleshooting"></a>

This chapter provides a table of the most common Elastic Beanstalk issues and how to resolve or work around them\. Error messages can appear as events on the environment management page in the console, in logs, or on the health page\.

If the health of your environment changes to red, try the following:
+ Review recent environment [events](using-features.events.md)\. Messages from Elastic Beanstalk about deployment, load, and configuration issues often appear here\.
+ [Pull logs](using-features.logging.md) to view recent log file entries\. Web server logs contain information about incoming requests and errors\.
+ [Connect to an instance](using-features.ec2connect.md) and check system resources\.
+ [Roll back](using-features.deploy-existing-version.md) to a previous, working version of the application\.
+ Undo recent configuration changes or restore a [saved configuration](environment-configuration-methods-before.md#configuration-options-before-savedconfig)\.
+ Deploy a new environment\. If it appears healthy, perform a [CNAME swap](using-features.CNAMESwap.md) to route traffic to the new environment and continue to debug the old one\.

**Topics**
+ [Connectivity](troubleshooting-connectivity.md)
+ [Environment creation and instance launches](troubleshooting-envcreate.md)
+ [Deployments](troubleshooting-deployments.md)
+ [Health](troubleshooting-health.md)
+ [Configuration](troubleshooting-configuration.md)
+ [Troubleshooting Docker containers](troubleshooting-docker.md)
+ [FAQ](troubleshooting-faq.md)