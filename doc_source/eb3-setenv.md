# `eb setenv`<a name="eb3-setenv"></a>

## Description<a name="eb3-setenv-description"></a>

Sets environment properties for the default environment\.

## Syntax<a name="eb3-setenv-syntax"></a>

`eb setenv key=value` 

You can include as many properties as you want, but the total size of all properties cannot exceed 4096 bytes\. You can delete a variable by leaving the value blank\. See  for limits\.

**Note**  
If the `value` contains a [special character](http://tldp.org/LDP/abs/html/special-chars.html), you must escape that character by preceeding it with a `\` character\.

## Options<a name="eb3-setenvoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  \-\-timeout  |  The number of minutes before the command times out\.  | 
|  Common options  |  | 

## Output<a name="eb3-setenv-output"></a>

If successful, the command displays that the environment update succeeded\.

## Example<a name="eb3-setenv-example"></a>

The following example sets the environment variable ExampleVar\.

```
$ eb setenv ExampleVar=ExampleValue
INFO: Environment update is starting.
INFO: Updating environment tmp-dev's configuration settings.
INFO: Successfully deployed new configuration to environment.
INFO: Environment update completed successfully.
```

The following command sets multiple environment properties\. It adds the environment property named `foo` and sets its value to `bar`, changes the value of the `JDBC_CONNECTION_STRING` property, and deletes the `PARAM4` and `PARAM5` properties\.

```
$ eb setenv foo=bar JDBC_CONNECTION_STRING=hello PARAM4= PARAM5=
```