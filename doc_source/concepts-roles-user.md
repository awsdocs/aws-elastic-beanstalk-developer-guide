# Elastic Beanstalk user policy<a name="concepts-roles-user"></a>

Create IAM users for each person who uses Elastic Beanstalk to avoid using your root account or sharing credentials\. For increased security, only grant these users permission to access services and features that they need\.

Elastic Beanstalk requires permissions not only for its own API actions, but for several other AWS services as well\. Elastic Beanstalk uses user permissions to launch all of the resources in an environment, including EC2 instances, an Elastic Load Balancing load balancer, and an Auto Scaling group\. Elastic Beanstalk also uses user permissions to save logs and templates to Amazon Simple Storage Service \(Amazon S3\), send notifications to Amazon SNS, assign instance profiles, and publish metrics to CloudWatch\. Elastic Beanstalk requires AWS CloudFormation permissions to orchestrate resource deployments and updates\. It also requires Amazon RDS permissions to create databases when needed, and Amazon SQS permissions to create queues for worker environments\.

For more information about user policies, see [Managing Elastic Beanstalk user policies](AWSHowTo.iam.managed-policies.md)\.