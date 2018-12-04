# eb swap<a name="eb3-swap"></a>

## Description<a name="eb3-swapdescription"></a>

Swaps the environment's CNAME with the CNAME of another environment \(for example, to avoid downtime when you update your application version\)\.

**Note**  
If you have more than two environments, you are prompted to select the name of the environment that is currently using your desired CNAME from a list of environments\. To suppress this, you can specify the name of the environment to use by including the `-n` option when you run the command\.

## Syntax<a name="eb3-swapsyntax"></a>

 eb swap 

 eb swap *environment\-name* 

**Note**  
The *environment\-name* is the environment for which you want a different CNAME\. If you don't specify *environment\-name* as a command line parameter when you run eb swap, EB CLI updates the CNAME of the default environment\.

## Options<a name="eb3-swapoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-n` or `--destination_name`  |  Specifies the name of the environment with which you want to swap CNAMEs\. If you run eb swap without this option, then EB CLI prompts you to choose from a list of your environments\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-swapoutput"></a>

If successful, the command returns the status of the `swap` operation\.

## Examples<a name="eb3-swapexample"></a>

The following example swaps the environment tmp\-dev with live\-env\.

```
$ eb swap
Select an environment to swap with.
1) staging-dev
2) live-env
(default is 1): 2
2018-07-11 21:05:25    INFO: swapEnvironmentCNAMEs is starting.
2018-07-11 21:05:26    INFO: Swapping CNAMEs for environments 'tmp-dev' and 'live-env'.
2018-07-11 21:05:30    INFO: 'tmp-dev.elasticbeanstalk.com' now points to 'awseb-e-j-AWSEBLoa-M7U21VXNLWHN-487871449.us-west-2.elb.amazonaws.com'.
2018-07-11 21:05:30    INFO: Completed swapping CNAMEs for environments 'tmp-dev' and 'live-env'.
```

The following example swaps the environment tmp\-dev with the environment live\-env but does not prompt you to enter or select a value for any settings\.

```
$ eb swap tmp-dev --destination_name live-env
2018-07-11 21:18:12    INFO: swapEnvironmentCNAMEs is starting.
2018-07-11 21:18:13    INFO: Swapping CNAMEs for environments 'tmp-dev' and 'live-env'.
2018-07-11 21:18:17    INFO: 'tmp-dev.elasticbeanstalk.com' now points to 'awseb-e-j-AWSEBLoa-M7U21VXNLWHN-487871449.us-west-2.elb.amazonaws.com'.
2018-07-11 21:18:17    INFO: Completed swapping CNAMEs for environments 'tmp-dev' and 'live-env'.
```