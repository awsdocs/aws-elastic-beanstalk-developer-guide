# Managing Multiple AWS Elastic Beanstalk Environments as a Group with the EB CLI<a name="ebcli-compose"></a>

You can use the EB CLI to create groups of environments, each running a separate component of a service\-oriented architecture application by using the `Compose Environments` API\.

Organize your application components into the following folder structure:

```
~/project-name
|-- component-a
|   `-- env.yaml
`-- component-b
    `-- env.yaml
```

Each subfolder contains the source code for an independent component of an application that will run in its own environment and an environment definition file named `env.yaml`\. For details on the `env.yaml` format, see \. 

To use the `Compose Environments` API, first run `eb init` from the project folder, specifying each component by the name of the folder that contains it with the `--modules` option:

```
~/workspace/project-name$ eb init --modules component-a component-b
```

The EB CLI prompts you to configure each component, and then creates the `.elasticbeanstalk` directory in each component folder\. EB CLI doesn't create configuration files in the parent directory\.

```
~/project-name
|-- component-a
|   |-- .elasticbeanstalk
|   `-- env.yaml
`-- component-b
    |-- .elasticbeanstalk
    `-- env.yaml
```

Next, run the `eb create` command with a list of environments to create, one for each component:

```
~/workspace/project-name$ eb create --modules component-a component-b --env-group-suffix group-name
```

This command creates an environment for each component\. The names of the environments are created by concatenating the `EnvironmentName` specified in the `env.yaml` file with the group name, separated by a hyphen\. The total length of these two options and the hyphen must not exceed the maximum allowed environment name length of 23 characters\.

To update the environment, use the `eb deploy` command:

```
~/workspace/project-name$ eb deploy --modules component-a component-b
```

You can update each component individually or you can update them as a group\. Specify the components that you want to update with the `--modules` option\.

The EB CLI stores the group name that you used with `eb create` in the `branch-defaults` section of the EB CLI configuration file under `/.elasticbeanstalk/config.yml`\. To deploy your application to a different group, use the `--env-group-suffix` option when you run `eb deploy`\. If the group does not already exist, the EB CLI will create a new group of environments:

```
~/workspace/project-name$ eb deploy --modules component-a component-b --env-group-suffix group-2-name
```

To terminate environments, run `eb terminate` in the folder for each module\. By default, the EB CLI will show an error if you try to terminate an environment that another running environment is dependent on\. Terminate the dependent environment first, or use the `--ignore-links` option to override the default behavior:

```
~/workspace/project-name/component-b$ eb terminate --ignore-links
```