# Troubleshooting Docker containers<a name="troubleshooting-docker"></a>

**Event:** *Failed to pull Docker image :latest: Invalid repository name \(\), only \[a\-z0\-9\-\_\.\] are allowed\. Tail the logs for more details\.*

Check the syntax of the `dockerrun.aws.json` file using a JSON validator\. Also verify the dockerfile contents against the requirements described in [Docker configuration](single-container-docker-configuration.md)

**Event:** *No EXPOSE directive found in Dockerfile, abort deployment*

The `Dockerfile` or the `dockerrun.aws.json` file does not declare the container port\. Use the `EXPOSE` instruction \(`Dockerfile`\) or `Ports` block \(`dockerrun.aws.json` file\) to expose a port for incoming traffic\.

**Event:** *Failed to download authentication credentials *repository* from *bucket name**

The `dockerrun.aws.json` provides an invalid EC2 key pair and/or S3 bucket for the `.dockercfg` file\. Or, the instance profile does not have GetObject authorization for the S3 bucket\. Verify that the `.dockercfg` file contains a valid S3 bucket and EC2 key pair\. Grant permissions for the action `s3:GetObject` to the IAM role in the instance profile\. For details, go to [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)

**Event:** *Activity execution failed, because: WARNING: Invalid auth configuration file*

Your authentication file \(`config.json`\) is not formatted correctly\. See [Using images from a private repository](create_deploy_docker.container.console.md#docker-images-private)