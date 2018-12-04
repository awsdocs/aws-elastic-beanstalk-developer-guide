# eb tags<a name="eb3-tags"></a>

## Description<a name="eb3-tagsdescription"></a>

Add, delete, update, and list Elastic Beanstalk environment tags\.

For details about environment tagging, see [Tagging Resources in Your Elastic Beanstalk Environment](using-features.tagging.md)\.

## Syntax<a name="eb3-tagsyntax"></a>

eb tags \[*environment\-name*\] \-l\|\-\-list

eb tags \[*environment\-name*\] \-a\|\-\-add *key1*=*value1*\[,*key2*=*value2* \.\.\.\]

eb tags \[*environment\-name*\] \-u\|\-\-update *key1*=*value1*\[,*key2*=*value2* \.\.\.\]

eb tags \[*environment\-name*\] \-d\|\-\-delete *key1*\[,*key2* \.\.\.\]

You can combine the `--add`, `--update`, and `--delete` subcommand options in a single command\. At least one of them is required\. You can't combined any of these three subcommand options with `--list`\.

## Options<a name="eb3-tagsoptions"></a>

None of these options are required\. If you run eb create without any options, you are prompted to enter or select a value for each setting\.


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-l` or `--list`  |  List all tags that are currently applied to the environment\.  | 
|  `-﻿a key1=value1[,key2=value2 ...]` or `-﻿-﻿add key1=value1[,key2=value2 ...]`  |  Apply new tags to the environment\. Specify tags as a comma\-separated list of `key=value` pairs\. You can't specify keys of existing tags\. Valid values: See [Tag an Environment](using-features.tagging.md)\.  | 
|  `-﻿u key1=value1[,key2=value2 ...]` or `-﻿-﻿update key1=value1[,key2=value2 ...]`  |  Update the values of existing environment tags\. Specify tags as a comma\-separated list of `key=value` pairs\. You must specify keys of existing tags\. Valid values: See [Tag an Environment](using-features.tagging.md)\.  | 
|  `-﻿d key1[,key2 ...]` or `-﻿-﻿delete key1[,key2 ...]`  |  Delete existing environment tags\. Specify tags as a comma\-separated list of keys\. You must specify keys of existing tags\. Valid values: See [Tag an Environment](using-features.tagging.md)\.  | 
|  `-r` *region* or `--region` *region*  |  The AWS Region in which your environment is running\. Default: the configured default region\. For the list of values you can specify for this option, see [AWS Elastic Beanstalk](http://docs.aws.amazon.com/general/latest/gr/rande.html#elasticbeanstalk_region) in [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.  | 

## Output<a name="eb3-tagsoutput"></a>

The `--list` subcommand option displays a list of the environments tags\. The output shows both the tags that Elastic Beanstalk applies by default and your custom tags\.

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

The following command adds a tag with the key `tag1` and the value `value1`, and at the same time deletes the tag `tag2`\.

```
$ eb tags --add tag1=value1 --delete tag2
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