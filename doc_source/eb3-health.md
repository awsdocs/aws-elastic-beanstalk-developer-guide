# eb health<a name="eb3-health"></a>

## Description<a name="eb3-healthdescription"></a>

Returns the most recent health for the environment\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command also returns the most recent health for the builder environment\.

## Syntax<a name="eb3-healthsyntax"></a>

 eb health 

 eb health *environment\-name* 

## Options<a name="eb3-healthoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-r` or `--refresh`  |  Show health information interactively and update every 10 seconds as new information is reported\.  | 
| \-\-mono | Don't display color in output\. | 

## Output<a name="eb3-healthoutput"></a>

If successful, the command returns recent health\.

## Example<a name="eb3-healthexample"></a>

The following example returns the most recent health information for a Linux environment\.

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