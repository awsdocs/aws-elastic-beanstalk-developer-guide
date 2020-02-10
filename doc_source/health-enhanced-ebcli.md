# Using the EB CLI to monitor environment health<a name="health-enhanced-ebcli"></a>

The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) is a command line tool for managing AWS Elastic Beanstalk environments\. You also can use the EB CLI to monitor your environment's health in real time and with more granularity than is currently available in the Elastic Beanstalk console

After [installing](eb-cli3-install.md) and [configuring](eb-cli3-configuration.md) the EB CLI, you can [launch a new environment](eb-cli3-getting-started.md) and deploy your code to it with the eb create command\. If you already have an environment that you created in the Elastic Beanstalk console, you can attach the EB CLI to it by running eb init in a project folder and following the prompts \(the project folder can be empty\)\. 

**Important**  
Ensure that you are using the latest version of the EB CLI by running `pip install` with the `--upgrade` option:  

```
$ sudo pip install --upgrade awsebcli
```
For complete EB CLI installation instructions, see [Install the EB CLI](eb-cli3-install.md)\.

To use the EB CLI to monitor your environment's health, you must first configure a local project folder by running eb init and following the prompts\. For complete instructions, see [Configure the EB CLI](eb-cli3-configuration.md)\.

If you already have an environment running in Elastic Beanstalk and want to use the EB CLI to monitor its health, follow these steps to attach it to the existing environment\.

**To attach the EB CLI to an existing environment**

1. Open a command line terminal and navigate to your user folder\.

1. Create and open a new folder for your environment\.

1. Run the eb init command, and then choose the application and environment whose health you want to monitor\. If you have only one environment running the application you choose, the EB CLI will select it automatically and you won't need to choose the environment, as shown in the following example\.

   ```
   ~/project$ eb init
   Select an application to use
   1) elastic-beanstalk-example
   2) [ Create new Application ]
   (default is 2): 1
   Select the default environment.
   You can change this later by typing "eb use [environment_name]".
   1) elasticBeanstalkEx2-env
   2) elasticBeanstalkExa-env
   (default is 1): 1
   ```

**To monitor health by using the EB CLI**

1. Open a command line and navigate to your project folder\.

1. Run the eb health command to display the health status of the instances in your environment\. In this example, there are five instances running in a Linux environment\.

   ```
   ~/project $ eb health
    elasticBeanstalkExa-env                                  Ok                       2015-07-08 23:13:20
   WebServer                                                                              Ruby 2.1 (Puma)
     total      ok    warning  degraded  severe    info   pending  unknown
       5        5        0        0        0        0        0        0
   
     instance-id   status     cause                                                                                                health
       Overall     Ok
     i-d581497d    Ok
     i-d481497c    Ok
     i-136e00c0    Ok
     i-126e00c1    Ok
     i-8b2cf575    Ok
   
     instance-id   r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10                                 requests
       Overall     671.8   100.0    0.0    0.0    0.0    0.003    0.002    0.001   0.001   0.000
     i-d581497d    143.0    1430      0      0      0    0.003    0.002    0.001   0.001   0.000
     i-d481497c    128.8    1288      0      0      0    0.003    0.002    0.001   0.001   0.000
     i-136e00c0    125.4    1254      0      0      0    0.004    0.002    0.001   0.001   0.000
     i-126e00c1    133.4    1334      0      0      0    0.003    0.002    0.001   0.001   0.000
     i-8b2cf575    141.2    1412      0      0      0    0.003    0.002    0.001   0.001   0.000
   
     instance-id   type       az   running     load 1  load 5      user%  nice%  system%  idle%   iowait%                             cpu
     i-d581497d    t2.micro   1a   12 mins        0.0    0.04        6.2    0.0      1.0   92.5       0.1
     i-d481497c    t2.micro   1a   12 mins       0.01    0.09        5.9    0.0      1.6   92.4       0.1
     i-136e00c0    t2.micro   1b   12 mins       0.15    0.07        5.5    0.0      0.9   93.2       0.0
     i-126e00c1    t2.micro   1b   12 mins       0.17    0.14        5.7    0.0      1.4   92.7       0.1
     i-8b2cf575    t2.micro   1c   1 hour        0.19    0.08        6.5    0.0      1.2   92.1       0.1
     
     instance-id   status     id   version              ago                                                                   deployments
     i-d581497d    Deployed   1    Sample Application   12 mins
     i-d481497c    Deployed   1    Sample Application   12 mins
     i-136e00c0    Deployed   1    Sample Application   12 mins
     i-126e00c1    Deployed   1    Sample Application   12 mins
     i-8b2cf575    Deployed   1    Sample Application   1 hour
   ```

   In this example, there is a single instance running in a Windows environment\.

   ```
   ~/project $ eb health
    WindowsSampleApp-env                                 Ok                                 2018-05-22 17:33:19
   WebServer                                                IIS 10.0 running on 64bit Windows Server 2016/2.2.0
     total      ok    warning  degraded  severe    info   pending  unknown
       1        1        0        0        0        0        0        0
   
     instance-id           status     cause                                                                                        health
       Overall             Ok
     i-065716fba0e08a351   Ok
   
     instance-id           r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10                         requests
       Overall              13.7   100.0    0.0    0.0    0.0    1.403    0.970    0.710   0.413   0.079
     i-065716fba0e08a351     2.4   100.0    0.0    0.0    0.0    1.102*   0.865    0.601   0.413   0.091
   
     instance-id           type       az   running     % user time    % privileged time  % idle time                                  cpu
     i-065716fba0e08a351   t2.large   1b   4 hours             0.2                  0.1         99.7
   
     instance-id           status     id   version              ago                                                           deployments
     i-065716fba0e08a351   Deployed   2    Sample Application   4 hours
   ```

## Reading the output<a name="health-enhanced-ebcli-output"></a>

The output displays the name of the environment, the environment's overall health, and the current date at the top of the screen\.

```
elasticBeanstalkExa-env                                  Ok                       2015-07-08 23:13:20
```

The next three lines display the type of environment \("WebServer" in this case\), the configuration \(Ruby 2\.1 with Puma\), and a breakdown of how many instances are in each of the seven states\.

```
WebServer                                                                              Ruby 2.1 (Puma)
  total      ok    warning  degraded  severe    info   pending  unknown
    5        5        0        0        0        0        0        0
```

The rest of the output is split into four sections\. The first displays the *status* and the *cause* of the status for the environment overall, and then for each instance\. The following example shows two instances in the environment with a status of `Info` and a cause indicating that a deployment has started\.

```
  instance-id    status     cause                                                                                                health
    Overall      Ok
  i-d581497d     Info       Performing application deployment (running for 3 seconds)
  i-d481497c     Info       Performing application deployment (running for 3 seconds)
  i-136e00c0     Ok
  i-126e00c1     Ok
  i-8b2cf575     Ok
```

For information about health statuses and colors, see [Health colors and statuses](health-enhanced-status.md)\.

The **requests** section displays information from the web server logs on each instance\. In this example, each instance is taking requests normally and there are no errors\.

```
  instance-id    r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10                                 requests
    Overall      13.7    100.0    0.0    0.0    0.0    1.403    0.970    0.710   0.413   0.079
  i-d581497d     2.4     100.0    0.0    0.0    0.0    1.102*   0.865    0.601   0.413   0.091
  i-d481497c     2.7     100.0    0.0    0.0    0.0    0.842*   0.788    0.480   0.305   0.062
  i-136e00c0     4.1     100.0    0.0    0.0    0.0    1.520*   1.088    0.883   0.524   0.104
  i-126e00c1     2.2     100.0    0.0    0.0    0.0    1.334*   0.791    0.760   0.344   0.197
  i-8b2cf575     2.3     100.0    0.0    0.0    0.0    1.162*   0.867    0.698   0.477   0.076
```

The **cpu** section shows operating system metrics for each instance\. The output differs by operating system\. Here is the output for Linux environments\.

```
  instance-id   type       az   running     load 1  load 5      user%  nice%  system%  idle%   iowait%                             cpu
  i-d581497d    t2.micro   1a   12 mins        0.0    0.03        0.2    0.0      0.0   99.7       0.1
  i-d481497c    t2.micro   1a   12 mins        0.0    0.03        0.3    0.0      0.0   99.7       0.0
  i-136e00c0    t2.micro   1b   12 mins        0.0    0.04        0.1    0.0      0.0   99.9       0.0
  i-126e00c1    t2.micro   1b   12 mins       0.01    0.04        0.2    0.0      0.0   99.7       0.1
  i-8b2cf575    t2.micro   1c   1 hour         0.0    0.01        0.2    0.0      0.1   99.6       0.1
```

Here is the output for Windows environments\.

```
  instance-id           type       az   running     % user time    % privileged time  % idle time
  i-065716fba0e08a351   t2.large   1b   4 hours             0.2                  0.0         99.8
```

For information about the server and operating system metrics shown, see [Instance metrics](health-enhanced-metrics.md)\.

The final section, **deployments**, shows the deployment status of each instance\. If a rolling deployment fails, you can use the deployment ID, status, and version label shown to identify instances in your environment that are running the wrong version\.

```
  instance-id   status     id   version              ago                                                                   deployments
  i-d581497d    Deployed   1    Sample Application   12 mins
  i-d481497c    Deployed   1    Sample Application   12 mins
  i-136e00c0    Deployed   1    Sample Application   12 mins
  i-126e00c1    Deployed   1    Sample Application   12 mins
  i-8b2cf575    Deployed   1    Sample Application   1 hour
```

## Interactive health view<a name="health-enhanced-ebcli-interactive"></a>

The eb health command displays a snapshot of your environment's health\. To refresh the displayed information every 10 seconds, use the `--refresh` option\.

```
$ eb health --refresh
 elasticBeanstalkExa-env                             Ok                            2015-07-09 22:10:04 (1 secs)
WebServer                                                                                        Ruby 2.1 (Puma)
  total      ok    warning  degraded  severe    info   pending  unknown
    5        5        0        0        0        0        0        0

  instance-id   status     cause                                                                                                health
    Overall     Ok
  i-bb65c145    Ok         Application deployment completed 35 seconds ago and took 26 seconds
  i-ba65c144    Ok         Application deployment completed 17 seconds ago and took 25 seconds
  i-f6a2d525    Ok         Application deployment completed 53 seconds ago and took 26 seconds
  i-e8a2d53b    Ok         Application deployment completed 32 seconds ago and took 31 seconds
  i-e81cca40    Ok

  instance-id   r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10                                 requests
    Overall     671.8   100.0    0.0    0.0    0.0    0.003    0.002    0.001   0.001   0.000
  i-bb65c145    143.0    1430      0      0      0    0.003    0.002    0.001   0.001   0.000
  i-ba65c144    128.8    1288      0      0      0    0.003    0.002    0.001   0.001   0.000
  i-f6a2d525    125.4    1254      0      0      0    0.004    0.002    0.001   0.001   0.000
  i-e8a2d53b    133.4    1334      0      0      0    0.003    0.002    0.001   0.001   0.000
  i-e81cca40    141.2    1412      0      0      0    0.003    0.002    0.001   0.001   0.000

  instance-id   type       az   running     load 1  load 5      user%  nice%  system%  idle%   iowait%                             cpu
  i-bb65c145    t2.micro   1a   12 mins        0.0    0.03        0.2    0.0      0.0   99.7       0.1
  i-ba65c144    t2.micro   1a   12 mins        0.0    0.03        0.3    0.0      0.0   99.7       0.0
  i-f6a2d525    t2.micro   1b   12 mins        0.0    0.04        0.1    0.0      0.0   99.9       0.0
  i-e8a2d53b    t2.micro   1b   12 mins       0.01    0.04        0.2    0.0      0.0   99.7       0.1
  i-e81cca40    t2.micro   1c   1 hour         0.0    0.01        0.2    0.0      0.1   99.6       0.1

  instance-id   status     id   version              ago                                                                   deployments
  i-bb65c145    Deployed   1    Sample Application   12 mins
  i-ba65c144    Deployed   1    Sample Application   12 mins
  i-f6a2d525    Deployed   1    Sample Application   12 mins
  i-e8a2d53b    Deployed   1    Sample Application   12 mins
  i-e81cca40    Deployed   1    Sample Application   1 hour

 (Commands: Help,Quit, ▼ ▲ ◄ ►)
```

This example shows an environment that has recently been scaled up from one to five instances\. The scaling operation succeeded, and all instances are now passing health checks and are ready to take requests\. In interactive mode, the health status updates every 10 seconds\. In the upper\-right corner, a timer ticks down to the next update\.

In the lower\-left corner, the report displays a list of options\. To exit interactive mode, press **Q**\. To scroll, press the arrow keys\. To see a list of additional commands, press **H**\.

## Interactive health view options<a name="health-enhanced-ebcli-options"></a>

When viewing environment health interactively, you can use keyboard keys to adjust the view and tell Elastic Beanstalk to replace or reboot individual instances\. To see a list of available commands while viewing the health report in interactive mode, press **H** \.

```
  up,down,home,end   Scroll vertically
  left,right         Scroll horizontally
  F                  Freeze/unfreeze data
  X                  Replace instance
  B                  Reboot instance
  <,>                Move sort column left/right
  -,+                Sort order descending/ascending
  P                  Save health snapshot data file
  Z                  Toggle color/mono mode
  Q                  Quit this program

  Views
  1                  All tables/split view
  2                  Status Table
  3                  Request Summary Table
  4                  CPU%/Load Table
  H                  This help menu


(press Q or ESC to return)
```