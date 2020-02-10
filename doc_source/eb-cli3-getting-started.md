# Managing Elastic Beanstalk environments with the EB CLI<a name="eb-cli3-getting-started"></a>

After [installing the EB CLI](eb-cli3-install.md) and [configuring your project directory](eb-cli3-configuration.md), you are ready to create an Elastic Beanstalk environment using the EB CLI, deploy source and configuration updates, and pull logs and events\.

**Note**  
Creating environments with the EB CLI requires a [service role](concepts-roles-service.md)\. You can create a service role by creating an environment in the Elastic Beanstalk console\. If you don't have a service role, the EB CLI attempts to create one when you run `eb create`\.

The EB CLI returns a zero \(`0`\) exit code for all successful commands, and a non\-zero exit code when it encounters any error\.

The following examples use an empty project folder named `eb` that was initialized with the EB CLI for use with a sample Docker application\.

**Topics**
+ [Eb create](#ebcli3-basics-create)
+ [Eb status](#ebcli3-basics-status)
+ [Eb health](#ebcli3-basics-health)
+ [Eb events](#ebcli3-basics-events)
+ [Eb logs](#ebcli3-basics-logs)
+ [Eb open](#ebcli3-basics-open)
+ [Eb deploy](#ebcli3-basics-deploy)
+ [Eb config](#ebcli3-basics-config)
+ [Eb terminate](#ebcli3-basics-terminate)

## Eb create<a name="ebcli3-basics-create"></a>

To create your first environment, run [eb create](eb3-create.md) and follow the prompts\. If your project directory has source code in it, the EB CLI will bundle it up and deploy it to your environment\. Otherwise, a sample application will be used\.

```
~/eb$ eb create
Enter Environment Name
(default is eb-dev): eb-dev
Enter DNS CNAME prefix
(default is eb-dev): eb-dev
WARNING: The current directory does not contain any source code. Elastic Beanstalk is launching the sample application instead.
Environment details for: elasticBeanstalkExa-env
  Application name: elastic-beanstalk-example
  Region: us-west-2
  Deployed Version: Sample Application
  Environment ID: e-j3pmc8tscn
  Platform: 64bit Amazon Linux 2015.03 v1.4.3 running Docker 1.6.2
  Tier: WebServer-Standard
  CNAME: eb-dev.elasticbeanstalk.com
  Updated: 2015-06-27 01:02:24.813000+00:00
Printing Status:
INFO: createEnvironment is starting.
 -- Events -- (safe to Ctrl+C) Use "eb abort" to cancel the command.
```

Your environment can take several minutes to become ready\. Press **Ctrl\+C** to return to the command line while the environment is created\.

## Eb status<a name="ebcli3-basics-status"></a>

Run eb status to see the current status of your environment\. When the status is `ready`, the sample application is available at elasticbeanstalk\.com and the environment is ready to be updated\.

```
~/eb$ eb status
Environment details for: elasticBeanstalkExa-env
  Application name: elastic-beanstalk-example
  Region: us-west-2
  Deployed Version: Sample Application
  Environment ID: e-gbzqc3jcra
  Platform: 64bit Amazon Linux 2015.03 v1.4.3 running Docker 1.6.2
  Tier: WebServer-Standard
  CNAME: elasticbeanstalkexa-env.elasticbeanstalk.com
  Updated: 2015-06-30 01:47:45.589000+00:00
  Status: Ready
  Health: Green
```

## Eb health<a name="ebcli3-basics-health"></a>

Use the eb health command to view [health information](health-enhanced.md) about the instances in your environment and the state of your environment overall\. Use the `--refresh` option to view health in an interactive view that updates every 10 seconds\.

```
~/eb$ eb health
 api                                    Ok                 2016-09-15 18:39:04
WebServer                                                               Java 8
  total      ok    warning  degraded  severe    info   pending  unknown
    3        3        0        0        0        0        0        0

  instance-id           status     cause                                health
    Overall             Ok
  i-0ef05ec54918bf567   Ok
  i-001880c1187493460   Ok
  i-04703409d90d7c353   Ok

  instance-id           r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10
    Overall             8.6     100.0    0.0    0.0    0.0    0.083*   0.065    0.053   0.040   0.019
  i-0ef05ec54918bf567   2.9        29      0      0      0    0.069*   0.066    0.057   0.050   0.023
  i-001880c1187493460   2.9        29      0      0      0    0.087*   0.069    0.056   0.050   0.034
  i-04703409d90d7c353   2.8        28      0      0      0    0.051*   0.027    0.024   0.021   0.015

  instance-id           type       az   running     load 1  load 5      user%  nice%  system%  idle%   iowait%
  i-0ef05ec54918bf567   t2.micro   1c   23 mins       0.19    0.05        3.0    0.0      0.3   96.7       0.0
  i-001880c1187493460   t2.micro   1a   23 mins        0.0     0.0        3.2    0.0      0.3   96.5       0.0
  i-04703409d90d7c353   t2.micro   1b   1 day          0.0     0.0        3.6    0.0      0.2   96.2       0.0

  instance-id           status     id   version                  ago                                deployments
  i-0ef05ec54918bf567   Deployed   28   app-bc1b-160915_181041   20 mins
  i-001880c1187493460   Deployed   28   app-bc1b-160915_181041   20 mins
  i-04703409d90d7c353   Deployed   28   app-bc1b-160915_181041   27 mins
```

## Eb events<a name="ebcli3-basics-events"></a>

Use eb events to see a list of events output by Elastic Beanstalk\.

```
~/eb$ eb events
2015-06-29 23:21:09    INFO    createEnvironment is starting.
2015-06-29 23:21:10    INFO    Using elasticbeanstalk-us-east-2-EXAMPLE as Amazon S3 storage bucket for environment data.
2015-06-29 23:21:23    INFO    Created load balancer named: awseb-e-g-AWSEBLoa-EXAMPLE
2015-06-29 23:21:42    INFO    Created security group named: awseb-e-gbzqc3jcra-stack-AWSEBSecurityGroup-EXAMPLE
...
```

## Eb logs<a name="ebcli3-basics-logs"></a>

Use eb logs to pull logs from an instance in your environment\. By default, eb logs pull logs from the first instance launched and displays them in standard output\. You can specify an instance ID with the \-\-instance option to get logs from a specific instance\.

The \-\-all option pulls logs from all instances and saves them to subdirectories under `.elasticbeanstalk/logs`\.

```
~/eb$ eb logs --all
Retrieving logs...
Logs were saved to /home/local/ANT/mwunderl/ebcli/environments/test/.elasticbeanstalk/logs/150630_201410
Updated symlink at /home/local/ANT/mwunderl/ebcli/environments/test/.elasticbeanstalk/logs/latest
```

## Eb open<a name="ebcli3-basics-open"></a>

To open your environment's website in a browser, use eb open:

```
~/eb$ eb open
```

In a windowed environment, your default browser will open in a new window\. In a terminal environment, a command line browser \(e\.g\. w3m\) will be used if available\.

## Eb deploy<a name="ebcli3-basics-deploy"></a>

Once the environment is up and ready, you can update it using eb deploy\.

This command works better with some source code to bundle up and deploy, so for this example we've created a `Dockerfile` in the project directory with the following content:

**\~/eb/Dockerfile**

```
FROM ubuntu:12.04

RUN apt-get update
RUN apt-get install -y nginx zip curl

RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN curl -o /usr/share/nginx/www/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master
RUN cd /usr/share/nginx/www/ && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip

EXPOSE 80

CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]
```

This `Dockerfile` deploys an image of Ubuntu 12\.04 and installs the game `2048`\. Run eb deploy to upload the application to your environment:

```
~/eb$ eb deploy
Creating application version archive "app-150630_014338".
Uploading elastic-beanstalk-example/app-150630_014338.zip to S3. This may take a while.
Upload Complete.
INFO: Environment update is starting.
 -- Events -- (safe to Ctrl+C) Use "eb abort" to cancel the command.
```

When you run eb deploy, the EB CLI bundles up the contents of your project directory and deploys it to your environment\. 

**Note**  
If you have initialized a git repository in your project folder, the EB CLI will always deploy the latest commit, even if you have pending changes\. Commit your changes prior to running eb deploy to deploy them to your environment\.

## Eb config<a name="ebcli3-basics-config"></a>

Take a look at the configuration options available for your running environment with the eb config command:

```
~/eb$ eb config
ApplicationName: elastic-beanstalk-example
DateUpdated: 2015-06-30 02:12:03+00:00
EnvironmentName: elasticBeanstalkExa-env
SolutionStackName: 64bit Amazon Linux 2015.03 v1.4.3 running Docker 1.6.2
settings:
  AWSEBAutoScalingScaleDownPolicy.aws:autoscaling:trigger:
    LowerBreachScaleIncrement: '-1'
  AWSEBAutoScalingScaleUpPolicy.aws:autoscaling:trigger:
    UpperBreachScaleIncrement: '1'
  AWSEBCloudwatchAlarmHigh.aws:autoscaling:trigger:
    UpperThreshold: '6000000'
...
```

This command populates a list of available configuration options in a text editor\. Many of the options shown have a `null` value, these are not set by default but can be modified to update the resources in your environment\. See [Configuration options](command-options.md) for more information about these options\.

## Eb terminate<a name="ebcli3-basics-terminate"></a>

If you are done using the environment for now, use eb terminate to terminate it\.

```
~/eb$ eb terminate
The environment "eb-dev" and all associated instances will be terminated.
To confirm, type the environment name: eb-dev
INFO: terminateEnvironment is starting.
INFO: Deleted CloudWatch alarm named: awseb-e-jc8t3pmscn-stack-AWSEBCloudwatchAlarmHigh-1XLMU7DNCBV6Y
INFO: Deleted CloudWatch alarm named: awseb-e-jc8t3pmscn-stack-AWSEBCloudwatchAlarmLow-8IVI04W2SCXS
INFO: Deleted Auto Scaling group policy named: arn:aws:autoscaling:us-east-2:123456789012:scalingPolicy:1753d43e-ae87-4df6-a405-11d31f4c8f97:autoScalingGroupName/awseb-e-jc8t3pmscn-stack-AWSEBAutoScalingGroup-90TTS2ZL4MXV:policyName/awseb-e-jc8t3pmscn-stack-AWSEBAutoScalingScaleUpPolicy-A070H1BMUQAJ
INFO: Deleted Auto Scaling group policy named: arn:aws:autoscaling:us-east-2:123456789012:scalingPolicy:1fd24ea4-3d6f-4373-affc-4912012092ba:autoScalingGroupName/awseb-e-jc8t3pmscn-stack-AWSEBAutoScalingGroup-90TTS2ZL4MXV:policyName/awseb-e-jc8t3pmscn-stack-AWSEBAutoScalingScaleDownPolicy-LSWFUMZ46H1V
INFO: Waiting for EC2 instances to terminate. This may take a few minutes.
 -- Events -- (safe to Ctrl+C)
```

For a full list of available EB CLI commands, check out the [EB CLI command reference](eb3-cmd-commands.md)\.