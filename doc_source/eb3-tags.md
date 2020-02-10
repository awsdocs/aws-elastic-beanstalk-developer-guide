# eb tags<a name="eb3-tags"></a>

## Description<a name="eb3-tagsdescription"></a>

Add, delete, update, and list tags of an Elastic Beanstalk resource\.

For details about resource tagging in Elastic Beanstalk, see [Tagging Elastic Beanstalk application resources](applications-tagging-resources.md)\.

## Syntax<a name="eb3-tagsyntax"></a>

eb tags \[*environment\-name*\] \[\-\-resource *ARN*\] \-l \| \-\-list

eb tags \[*environment\-name*\] \[\-\-resource *ARN*\] \-a \| \-\-add *key1*=*value1*\[,*key2*=*value2* \.\.\.\]

eb tags \[*environment\-name*\] \[\-\-resource *ARN*\] \-u \| \-\-update *key1*=*value1*\[,*key2*=*value2* \.\.\.\]

eb tags \[*environment\-name*\] \[\-\-resource *ARN*\] \-d \| \-\-delete *key1*\[,*key2* \.\.\.\]

You can combine the `--add`, `--update`, and `--delete` subcommand options in a single command\. At least one of them is required\. You can't combined any of these three subcommand options with `--list`\.

Without any additional arguments, all of these commands list or modify tags of the default environment in the current directory's application\. With an *environment\-name* argument, the commands list or modify tags of that environment\. With the `--resource` option, the commands list or modify tags of any Elastic Beanstalk resource – an application, an environment, an application version, a saved configuration, or a custom platform version\. Specify the resource by its Amazon Resource Name \(ARN\)\.

## Options<a name="eb3-tagsoptions"></a>

None of these options are required\. If you run eb create without any options, you are prompted to enter or select a value for each setting\.


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-l` or `--list`  |  List all tags that are currently applied to the resource\.  | 
|  `-﻿a key1=value1[,key2=value2 ...]` or `-﻿-﻿add key1=value1[,key2=value2 ...]`  |  Apply new tags to the resource\. Specify tags as a comma\-separated list of `key=value` pairs\. You can't specify keys of existing tags\. Valid values: See [Tagging resources](applications-tagging-resources.md)\.  | 
|  `-﻿u key1=value1[,key2=value2 ...]` or `-﻿-﻿update key1=value1[,key2=value2 ...]`  |  Update the values of existing resource tags\. Specify tags as a comma\-separated list of `key=value` pairs\. You must specify keys of existing tags\. Valid values: See [Tagging resources](applications-tagging-resources.md)\.  | 
|  `-﻿d key1[,key2 ...]` or `-﻿-﻿delete key1[,key2 ...]`  |  Delete existing resource tags\. Specify tags as a comma\-separated list of keys\. You must specify keys of existing tags\. Valid values: See [Tagging resources](applications-tagging-resources.md)\.  | 
|  `-r` *region* or `--region` *region*  |  The AWS Region in which your resource exists\. Default: the configured default region\. For the list of values you can specify for this option, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html) in the *AWS General Reference*\.  | 
|  `-﻿-﻿resource ARN`  |  The ARN of the resource that the command modifies or lists tags for\. If not specified, the command refers to the \(default or specified\) environment in the current directory's application\. Valid values: See one of the sub\-topic of [Tagging resources](applications-tagging-resources.md) that is specific to the resource you're interested in\. These topics show how the resource's ARN is constructed and explain how to get a list of this resource's ARNs that exist for your application or account\.  | 

## Output<a name="eb3-tagsoutput"></a>

The `--list` subcommand option displays a list of the resource's tags\. The output shows both the tags that Elastic Beanstalk applies by default and your custom tags\.

```
$ eb tags --list
Showing tags for environment 'MyApp-env':

Key                                 Value

Name                                MyApp-env
elasticbeanstalk:environment-id     e-63cmxwjaut
elasticbeanstalk:environment-name   MyApp-env
mytag                               tagvalue
tag2                                2nd value
```

The `--add`, `--update`, and `--delete` subcommand options, when successful, don't have any output\. You can add the `--verbose` option to see detailed output of the command's activity\.

```
$ eb tags --verbose --update "mytag=tag value"
Updated Tags:

Key                                 Value

mytag                               tag value
```

## Examples<a name="eb3-tagsexamples"></a>

The following command successfully adds a tag with the key `tag1` and the value `value1` to the application's default environment, and at the same time deletes the tag `tag2`\.

```
$ eb tags --add tag1=value1 --delete tag2
```

The following command successfully adds a tag to a saved configuration within an application\.

```
$ eb tags --add tag1=value1 \
      --resource "arn:aws:elasticbeanstalk:us-east-2:my-account-id:configurationtemplate/my-app/my-template"
```

The following command fails because it tries to update a nonexisting tag\.

```
$ eb tags --update tag3=newval
ERROR: Tags with the following keys can't be updated because they don't exist:

  tag3
```

The following command fails because it tries to update and delete the same key\.

```
$ eb tags --update mytag=newval --delete mytag
ERROR: A tag with the key 'mytag' is specified for both '--delete' and '--update'. Each tag can be either deleted or updated in a single operation.
```