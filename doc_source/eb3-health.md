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
|  `-r` or `--refresh`  |  Show health information interactively and update every ten seconds as new information is reported\.  | 
| \-\-mono | Don't display color in output\. | 

## Output<a name="eb3-healthoutput"></a>

If successful, the command returns recent health\.

## Example<a name="eb3-healthexample"></a>

The following example returns the most recent health\.

```
$ eb health
 elasticBeanstalkExa-env                                  Ok                       2015-07-08 23:13:20
WebServer                                                                              Ruby 2.1 (Puma)
  total      ok    warning  degraded  severe    info   pending  unknown
    5        5        0        0        0        0        0        0

  id             status     cause
    Overall      Ok
  i-d581497d     Ok
  i-d481497c     Ok
  i-136e00c0     Ok
  i-126e00c1     Ok
  i-8b2cf575     Ok

  id             r/sec    %2xx   %3xx   %4xx   %5xx      p99      p90      p75     p50     p10
    Overall      0.0         -      -      -      -         -        -       -       -       -
  i-d581497d     0.0         -      -      -      -         -        -       -       -       -
  i-d481497c     0.0         -      -      -      -         -        -       -       -       -
  i-136e00c0     0.0         -      -      -      -         -        -       -       -       -
  i-126e00c1     0.0         -      -      -      -         -        -       -       -       -
  i-8b2cf575     0.0         -      -      -      -         -        -       -       -       -

  instance-id   type       az   running     load 1  load 5      user%  nice%  system%  idle%   iowait%
  i-d581497d    t2.micro   1a   12 mins        0.0    0.03        0.2    0.0      0.0   99.7       0.1
  i-d481497c    t2.micro   1a   12 mins        0.0    0.03        0.3    0.0      0.0   99.7       0.0
  i-136e00c0    t2.micro   1b   12 mins        0.0    0.04        0.1    0.0      0.0   99.9       0.0
  i-126e00c1    t2.micro   1b   12 mins       0.01    0.04        0.2    0.0      0.0   99.7       0.1
  i-8b2cf575    t2.micro   1c   1 hour         0.0    0.01        0.2    0.0      0.1   99.6       0.1

  instance-id   status     id   version              ago                                  deployments
  i-d581497d    Deployed   1    Sample Application   12 mins
  i-d481497c    Deployed   1    Sample Application   12 mins
  i-136e00c0    Deployed   1    Sample Application   12 mins
  i-126e00c1    Deployed   1    Sample Application   12 mins
  i-8b2cf575    Deployed   1    Sample Application   1 hour
```