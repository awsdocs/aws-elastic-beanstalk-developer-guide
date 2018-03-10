# delete<a name="delete"></a>

**Note**  
 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see [The Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md)\. 

## Description<a name="deletedescription"></a>

Deletes the current application, or an application you specify, along with all associated environments, versions, and configurations\. For a tutorial that includes a description of how to use `eb delete` to delete an application, see [Getting Started with Eb](command-reference-get-started.md)\.

**Note**  
The `delete` operation applies to an application and all of its environments\. To stop only a single environment rather than an entire application, use ` eb [stop](stop.md)`\.

## Syntax<a name="deletesyntax"></a>

 **`eb delete`** 

## Options<a name="deleteoptions"></a>


****  

|  **Name**  |  **Description**  |  **Required**  | 
| --- | --- | --- | 
|  `-a` or `--application-name` *`APPLICATION_NAME`*   |  The application that you want to delete\. If you do not use this option, eb will delete the application currently specified in `.elasticbeanstalk/optionsettings`\. To verify your current settings, run `eb init` \(the current values will be displayed; press Enter at each prompt to keep the current value\)\. Type: String Default: *Current setting*  |  No  | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="deleteoutput"></a>

If successful, the command returns confirmation that the application was deleted\.

## Example<a name="deleteexample"></a>

The following example request deletes the specified application and all of its environments\. Replace the red placeholder text with your own values\.

```
PROMPT> delete -a MyApp

Delete application? [y/n]: y
Deleted application "MyApp".
```