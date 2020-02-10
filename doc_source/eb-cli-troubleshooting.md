# Troubleshooting issues with the EB CLI<a name="eb-cli-troubleshooting"></a>

This topic lists common error messages encountered when using the EB CLI and possible solutions\. If you encounter an error message not shown here, use the *Feedback* links to let us know about it\.

**ERROR: An error occurred while handling git command\. Error code: 128 Error: fatal: Not a valid object name HEAD**

**Cause:** This error message is shown when you have initialized a Git repository but have not yet committed\. The EB CLI looks for the HEAD revision when your project folder contains a Git repository\.

**Solution:** Add the files in your project folder to the staging area and commit:

```
~/my-app$ git add .
~/my-app$ git commit -m "First commit"
```

**ERROR: This branch does not have a default environment\. You must either specify an environment by typing "eb status my\-env\-name" or set a default environment by typing "eb use my\-env\-name"\. **

**Cause:** When you create a new branch in git, it is not attached to an Elastic Beanstalk environment by default\.

**Solution:** Run eb list to see a list of available environments\. Then run eb use *env\-name* to use one of the available environments\.

**ERROR: 2\.0\+ Platforms require a service role\. You can provide one with \-\-service\-role option**

**Cause:** If you specify an environment name with eb create \(for example, eb create my\-env\), the EB CLI will not attempt to create a service role for you\. If you don't have the default service role, the above error is shown\.

**Solution:** Run eb create without an environment name and follow the prompts to create the default service role\.

## Troubleshooting deployments<a name="python-common-troubleshooting"></a>

If your Elastic Beanstalk deployment didn't go quite as smoothly as planned, you may get a 404 \(if your application failed to launch\) or 500 \(if your application fails during runtime\) response, instead of seeing your website\. To troubleshoot many common issues, you can use the EB CLI to check the status of your deployment, view its logs, gain access to your EC2 instance with SSH, or to open the AWS Management Console page for your application environment\.

**To use the EB CLI to help troubleshoot your deployment**

1. Run eb status to see the status of your current deployment and health of your EC2 hosts\. For example:

   ```
   $ eb status --verbose
   
   Environment details for: python_eb_app
     Application name: python_eb_app
     Region: us-west-2
     Deployed Version: app-150206_035343
     Environment ID: e-wa8u6rrmqy
     Platform: 64bit Amazon Linux 2014.09 v1.1.0 running Python 2.7
     Tier: WebServer-Standard-
     CNAME: python_eb_app.elasticbeanstalk.com
     Updated: 2015-02-06 12:00:08.557000+00:00
     Status: Ready
     Health: Green
     Running instances: 1
         i-8000528c: InService
   ```
**Note**  
Using the `--verbose` switch provides information about the status of your running instances\. Without it, eb status will print only general information about your environment\.

1. Run eb health to view health information about your environment:

   ```
   $ eb health --refresh
    elasticBeanstalkExa-env                                  Degraded                  2016-03-28 23:13:20
   WebServer                                                                              Ruby 2.1 (Puma)
     total      ok    warning  degraded  severe    info   pending  unknown
       5        2        0        2        1        0        0        0
   
     instance-id   status     cause
       Overall     Degraded  Incorrect application version found on 3 out of 5 instances. Expected version "Sample Application" (deployment 1).
     i-d581497d    Degraded  Incorrect application version "v2" (deployment 2). Expected version "Sample Application" (deployment 1).
     i-d481497c    Degraded  Incorrect application version "v2" (deployment 2). Expected version "Sample Application" (deployment 1).
     i-136e00c0    Severe    Instance ELB health has not been available for 5 minutes.
     i-126e00c1    Ok
     i-8b2cf575    Ok
   
     instance-id   r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10
       Overall     646.7   100.0    0.0    0.0    0.0    0.003    0.002    0.001   0.001   0.000
     i-dac3f859    167.5    1675      0      0      0    0.003    0.002    0.001   0.001   0.000
     i-05013a81    161.2    1612      0      0      0    0.003    0.002    0.001   0.001   0.000
     i-04013a80    0.0         -      -      -      -         -        -       -       -       -
     i-3ab524a1    155.9    1559      0      0      0    0.003    0.002    0.001   0.001   0.000
     i-bf300d3c    162.1    1621      0      0      0    0.003    0.002    0.001   0.001   0.000
   
     instance-id   type       az   running     load 1  load 5      user%  nice%  system%  idle%   iowait%
     i-d581497d    t2.micro   1a   25 mins       0.16     0.1        7.0    0.0      1.7   91.0       0.1
     i-d481497c    t2.micro   1a   25 mins       0.14     0.1        7.2    0.0      1.6   91.1       0.0
     i-136e00c0    t2.micro   1b   25 mins        0.0    0.01        0.0    0.0      0.0   99.9       0.1
     i-126e00c1    t2.micro   1b   25 mins       0.03    0.08        6.9    0.0      2.1   90.7       0.1
     i-8b2cf575    t2.micro   1c   1 hour        0.05    0.41        6.9    0.0      2.0   90.9       0.0
     
     instance-id   status     id   version              ago                                  deployments
     i-d581497d    Deployed   2    v2                   9 mins
     i-d481497c    Deployed   2    v2                   7 mins
     i-136e00c0    Failed     2    v2                   5 mins
     i-126e00c1    Deployed   1    Sample Application   25 mins
     i-8b2cf575    Deployed   1    Sample Application   1 hour
   ```

   The above example shows an environment with five instances where the deployment of version "v2" failed on the third instance\. After a failed deployment, the expected version is reset to the last version that succeeded, which in this case is "Sample Application" from the first deployment\. See [Using the EB CLI to monitor environment health](health-enhanced-ebcli.md) for more information\.

1. Run eb logs to download and view the logs associated with your application deployment\.

   ```
   $ eb logs
   ```

1. Run eb ssh to connect with the EC2 instance that's running your application and examine it directly\. On the instance, your deployed application can be found in the `/opt/python/current/app` directory, and your Python environment will be found in `/opt/python/run/venv/`\.

1. Run eb console to view your application environment on the [AWS Management Console](https://aws.amazon.com/console/)\. You can use the web interface to easily examine various aspects of your deployment, including your application's configuration, status, events, logs\. You can also download the current or past application versions that you've deployed to the server\.