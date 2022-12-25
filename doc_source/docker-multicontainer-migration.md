# Migrating to the Docker Amazon Linux 2 Platform<a name="docker-multicontainer-migration"></a>

You can migrate your applications running on the [Multi\-container Docker platform on Amazon Linux AMI](create_deploy_docker_ecs.md) to the Amazon Linux 2 Docker platform\. The Multi\-container Docker platform on Amazon Linux AMI requires that you specify prebuilt application images to run as containers\. After migrating, you will no longer have this limitation, because the Amazon Linux 2 Docker platform also allows Elastic Beanstalk to build your container images during deployment\.

Your applications will continue to run in multi\-container environments with the added benefits from the Docker Compose tool\. To learn more about Docker Compose and how to install it, see the Docker sites [Overview of Docker Compose](https://docs.docker.com/compose/) and [Install Docker Compose](https://docs.docker.com/compose/install/)\.

## The `docker-compose.yml` file<a name="docker-multicontainer-migration.files"></a>

The Docker Compose tool uses the `docker-compose.yml` file for configuration of your application services\. This file replaces your `Dockerrun.aws.json v2` file in your application project directory and application source bundle\. You create the `docker-compose.yml` file manually, and will find it helpful to reference your `Dockerrun.aws.json v2` file for most of the parameter values\.

Below is an example of a `docker-compose.yml` file and the corresponding `Dockerrun.aws.json v2` file for the same application\. For more information on the `docker-compose.yml` file, see [Compose file reference](https://docs.docker.com/compose/compose-file/)\. For more information on the `Dockerrun.aws.json v2` file, see [`Dockerrun.aws.json` v2](create_deploy_docker_v2config.md#create_deploy_docker_v2config_dockerrun)\. 


| **`docker-compose.yml`** | **`Dockerrun.aws.json v2`** | 
| --- | --- | 
|   [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/docker-multicontainer-migration.html)   |   [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/docker-multicontainer-migration.html)   | 
|  <pre>version: '2.4'<br />services:<br />  php-app:<br />    image: "php:fpm"<br />    volumes:<br />      - "./php-app:/var/www/html:ro"<br />      - "${EB_LOG_BASE_DIR}/php-app:/var/log/sample-app"<br />    mem_limit: 128m<br />    environment:<br />      Container: PHP<br />  nginx-proxy:<br />    image: "nginx"<br />    ports:<br />      - "80:80"<br />    volumes:<br />      - "./php-app:/var/www/html:ro"<br />      - "./proxy/conf.d:/etc/nginx/conf.d:ro"<br />      - "${EB_LOG_BASE_DIR}/nginx-proxy:/var/log/nginx"<br />    mem_limit: 128m<br />    links:<br />      - php-app</pre>  | 
|  <pre>{<br />  "AWSEBDockerrunVersion": 2,<br />  "volumes": [<br />    {<br />      "name": "php-app",<br />      "host": {<br />        "sourcePath": "/var/app/current/php-app"<br />      }<br />    },<br />    {<br />      "name": "nginx-proxy-conf",<br />      "host": {<br />        "sourcePath": "/var/app/current/proxy/conf.d"<br />      }<br />    }<br />  ],<br />  "containerDefinitions": [<br />    {<br />      "name": "php-app",<br />      "image": "php:fpm",<br />      "environment": [<br />        {<br />          "name": "Container",<br />          "value": "PHP"<br />        }<br />      ],<br />      "essential": true,<br />      "memory": 128,<br />      "mountPoints": [<br />        {<br />          "sourceVolume": "php-app",<br />          "containerPath": "/var/www/html",<br />          "readOnly": true<br />        }<br />      ]<br />    },<br />    {<br />      "name": "nginx-proxy",<br />      "image": "nginx",<br />      "essential": true,<br />      "memory": 128,<br />      "portMappings": [<br />        {<br />          "hostPort": 80,<br />          "containerPort": 80<br />        }<br />      ],<br />      "links": [<br />        "php-app"<br />      ],<br />      "mountPoints": [<br />        {<br />          "sourceVolume": "php-app",<br />          "containerPath": "/var/www/html",<br />          "readOnly": true<br />        },<br />        {<br />          "sourceVolume": "nginx-proxy-conf",<br />          "containerPath": "/etc/nginx/conf.d",<br />          "readOnly": true<br />        },<br />        {<br />          "sourceVolume": "awseb-logs-nginx-proxy",<br />          "containerPath": "/var/log/nginx"<br />        }<br />      ]<br />    }<br />  ]<br />}<br /> </pre>  | 

## Additional Migration Considerations<a name="docker-multicontainer-migration.considerations"></a>

The Docker Amazon Linux 2 platform and Multi\-container Docker Amazon Linux AMI platform implement environment properties differently\. These two platforms also have different log directories that Elastic Beanstalk creates for each of their containers\. After you migrate from the Amazon Linux AMI Multi\-container Docker platform, you will need to be aware of these different implementations for your new Amazon Linux 2 Docker platform environment\.


|   **Area**  |   **Docker platform on Amazon Linux 2 with Docker Compose**  |   **Multi\-container Docker platform on Amazon Linux AMI**  | 
| --- | --- | --- | 
|  Environment properties  |   In order for your containers to access environment properties you must add a reference to the `.env` file in the `docker-compose.yml` file\. Elastic Beanstalk generates the `.env` file, listing each of the properties as environment variables\. For more information see [Referencing environment variables in containers](create_deploy_docker.container.console.md#docker-env-cfg.env-variables)\.   |   Elastic Beanstalk can directly pass environment properties to the container\. Your code running in the container can access these properties as environment variables without any additional configuration\.   | 
|   Log directories  |   For each container Elastic Beanstalk creates a log directory called `/var/log/eb-docker/containers/<service name>` \(or `${EB_LOG_BASE_DIR}/<service name>`\)\. For more information see [Docker container customized logging \(Docker Compose\)](create_deploy_docker.container.console.md#docker-env-cfg.dc-customized-logging)\.   |   For each container, Elastic Beanstalk creates a log directory called `/var/log/containers/<containername>`\. For more information see `mountPoints` field in [Container definition format](create_deploy_docker_v2config.md#create_deploy_docker_v2config_dockerrun_format)\.   | 

## Migration Steps<a name="docker-multicontainer-migration.procedure"></a>

**To migrate to the Amazon Linux 2 Docker platform**

1. Create the `docker-compose.yml ` file for your application, based on its existing `Dockerrun.aws.json v2` file\. For more information see the above section [The `docker-compose.yml` file](#docker-multicontainer-migration.files)\.

1. In your application project folder's root directory, replace the `Dockerrun.aws.json v2` file with the `docker-compose.yml` you just created\. 

   Your directory structure should be as follows\.

   ```
   ~/myApplication
   |-- docker-compose.yml
   |-- .ebextensions
   |-- php-app
   |-- proxy
   ```

1. Use the eb init command to configure your local directory for deployment to Elastic Beanstalk\.

   ```
   ~/myApplication$ eb init -p docker application-name
   ```

1. Use the eb create command to create an environment and deploy your Docker image\.

   ```
   ~/myApplication$ eb create environment-name
   ```

1. If your app is a web application, after your environment launches, use the eb open command to view it in a web browser\.

   ```
   ~/myApplication$ eb open environment-name
   ```

1. You can display the status of your newly created environment using the eb status command\.

   ```
   ~/myApplication$ eb status environment-name
   ```