# eb printenv<a name="eb3-printenv"></a>

## Description<a name="eb3-printenvdescription"></a>

Prints all the environment properties in the command window\.

## Syntax<a name="eb3-printenvsyntax"></a>

 eb printenv 

 eb printenv *environment\-name* 

## Options<a name="eb3-printenvoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-printenvoutput"></a>

If successful, the command returns the status of the `printenv` operation\.

## Example<a name="eb3-printenvexample"></a>

The following example prints environment properties for the specified environment\.

```
$ eb printenv
Environment Variables:
     PARAM1 = Value1
```