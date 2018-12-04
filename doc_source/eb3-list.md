# eb list<a name="eb3-list"></a>

## Description<a name="eb3-listdescription"></a>

Lists all environments in the current application or all environments in all applications, as specified by the `--all` option\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command also lists the builder environments\.

## Syntax<a name="eb3-listsyntax"></a>

 eb list 

## Options<a name="listoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-a` or `--all`  |  Lists all environments from all applications\.  | 
|  `-v` or `--verbose`  |  Provides more detailed information about all environments, including instances\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-listoutput"></a>

If successful, the command returns a list of environment names in which your current environment is marked with an asterisk \(\*\)\.

## Example 1<a name="eb3-listexample1"></a>

The following example lists your environments and indicates that tmp\-dev is your default environment\.

```
$ eb list
* tmp-dev
```

## Example 2<a name="eb3-listexample2"></a>

The following example lists your environments with additional details\.

```
$ eb list --verbose
Region: us-west-2
Application: tmp
    Environments: 1
        * tmp-dev : ['i-c7ee492d']
```