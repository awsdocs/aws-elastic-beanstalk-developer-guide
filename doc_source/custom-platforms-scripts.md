# Platform Scripts<a name="custom-platforms-scripts"></a>

Elastic Beanstalk installs the shell script `get-config` that you can use to get environment variables and other information in hooks that run on\-instance in environments launched with your custom platform\.

This tool is available at `/opt/elasticbeanstalk/bin/get-config`\. You can use it in the following ways:
+ `get-config optionsettings` – Returns a JSON object listing the configuration options set on the environment, organized by namespace\.

  ```
  $ /opt/elasticbeanstalk/bin/get-config optionsettings
  {"aws:elasticbeanstalk:container:php:phpini":{"memory_limit":"256M","max_execution_time":"60","display_errors":"Off","composer_options":"","allow_url_fopen":"On","zlib_output_compression":"Off","document_root":""},"aws:elasticbeanstalk:hostmanager":{"LogPublicationControl":"false"},"aws:elasticbeanstalk:application:environment":{"TESTPROPERTY":"testvalue"}}
  ```

  To return a specific configuration option, use the `-n` option to specify a namespace, and the `-o` option to specify an option name\.

  ```
  $ /opt/elasticbeanstalk/bin/get-config optionsettings -n aws:elasticbeanstalk:container:php:phpini -o memory_limit
  256M
  ```
+ `get-config environment` – Returns a JSON object containing a list of environment properties, including both user\-configured properties and those provided by Elastic Beanstalk\.

  ```
  $ /opt/elasticbeanstalk/bin/get-config environment
  {"TESTPROPERTY":"testvalue","RDS_PORT":"3306","RDS_HOSTNAME":"anj9aw1b0tbj6b.cijbpanmxz5u.us-west-2.rds.amazonaws.com","RDS_USERNAME":"testusername","RDS_DB_NAME":"ebdb","RDS_PASSWORD":"testpassword1923851"}
  ```

   For example, Elastic Beanstalk provides environment properties for connecting to an integrated RDS DB instance \(RDS\_HOSTNAME, etc\.\)\. These properties appear in the output of `get-config environment` but not in the output of `get-config optionsettings`, because they are not set by the user\.

  To return a specific environment property, use the `-k` option to specify a property key\.

  ```
  $ /opt/elasticbeanstalk/bin/get-config environment -k TESTPROPERTY
  testvalue
  ```

You can test the previous commands by using SSH to connect to an instance in an Elastic Beanstalk environment running a Linux\-based platform\.

See the following files in the [sample platform definition archive](custom-platforms.md#custom-platforms-sample) for an example of `get-config` usage:
+ `builder/platform-uploads/opt/elasticbeanstalk/hooks/configdeploy/enact/02-gen-envvars.sh` – Gets environment properties\.
+ `builder/platform-uploads/opt/SampleNodePlatform/bin/createPM2ProcessFile.js` – Parses the output\.

Elastic Beanstalk installs the shell script `download-source-bundle` that you can use to download your application source code during the deployment of your custom platform\. This tool is available at `/opt/elasticbeanstalk/bin/download-source-bundle`\. See the sample script `00-unzip.sh`, which is in the `appdeploy/pre` folder, for an example of how to use `download-source-bundle` to download application source code to the `/opt/elasticbeanstalk/deploy/appsource` folder during deployment\.