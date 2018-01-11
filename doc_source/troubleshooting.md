# Troubleshooting<a name="troubleshooting"></a>

This chapter provides a table of the most common Elastic Beanstalk issues and how to resolve or work around them\. Error messages can appear as events on the environment Dashboard in the console, in logs, or on the health page\.

If the health of your environment changes to red, try the following:

+ Review recent environment events\. Messages from Elastic Beanstalk about deployment, load, and configuration issues often appear here\.

+ Pull logs to view recent log file entries\. Web server logs contain information about incoming requests and errors\.

+ Connect to an instance and check system resources\.

+ Roll back to a previous, working version of the application\.

+ Undo recent configuration changes or restore a saved configuration\.

+ Deploy a new environment\. If it appears healthy, perform a CNAME swap to route traffic to the new environment and continue to debug the old one\.


+ [Connectivity](troubleshooting-connectivity.md)
+ [Environment Creation and Instance Launches](troubleshooting-envcreate.md)
+ [Deployments](troubleshooting-deployments.md)
+ [Health](troubleshooting-health.md)
+ [Configuration](troubleshooting-configuration.md)
+ [Troubleshooting Docker Containers](troubleshooting-docker.md)
+ [FAQ](troubleshooting-faq.md)