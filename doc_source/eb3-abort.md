# eb abort<a name="eb3-abort"></a>

## Description<a name="eb3-abortdescription"></a>

Cancels an upgrade when environment configuration changes to instances are still in progress\.

**Note**  
If you have more than two environments that are undergoing a update, you are prompted to select the name of the environment for which you want to roll back changes\.

## Syntax<a name="eb3-abortsyntax"></a>

 eb abort 

 eb abort *environment\-name* 

## Options<a name="eb3-abortoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-abortoutput"></a>

The command shows a list of environments currently being updated and prompts you to choose the update that you want to abort\. If only one environment is currently being updated, you do not need to specify the environment name\. If successful, the command reverts environment configuration changes\. The rollback process continues until all instances in the environment have the previous environment configuration or until the rollback process fails\.

## Example<a name="eb3-abortexample"></a>

The following example cancels the platform upgrade\.

```
$ eb abort
Aborting update to environment "tmp-dev".
<list of events>
```