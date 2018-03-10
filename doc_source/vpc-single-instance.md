# Example: Launching a Single\-Instance Environment without Any Associated Private Resources in a VPC<a name="vpc-single-instance"></a>

You can deploy an Elastic Beanstalk application in a public subnet\. Use this configuration if you just have a single instance without any private resources that are associated with the instance, such as an Amazon RDS DB instance that you don't want publicly accessible\. Elastic Beanstalk assigns an Elastic IP address to the instance so that it can access the Internet through the VPC Internet gateway\.

![\[Elastic Beanstalk and VPC Topology\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-pub-diagram.png)

To deploy a single\-instance Elastic Beanstalk application inside a VPC, you need to complete the following:


+ [Create a VPC with a Public Subnet](#vpc-single-instance-create)
+ [Deploy to Elastic Beanstalk](#vpc-single-instance-create-env)

## Create a VPC with a Public Subnet<a name="vpc-single-instance-create"></a>

**To create a VPC**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **VPC Dashboard** and then choose **Start VPC Wizard**\. 

1. Select **VPC with a Single Public Subnet** and then choose **Select**\.  
![\[Choose the first option in the wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-wiz-single.png)

   A confirmation page shows the CIDR blocks used for the VPC and subnet\. The page also shows the subnet and the associated Availability Zone\.  
![\[Option 1 confirmation page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-wiz-single-2.png)

1. Choose **Create VPC**\.

   AWS creates your VPC, subnet, Internet gateway, and route table\. Choose **OK** to exit the wizard\.

   After AWS successfully creates the VPC, it assigns the VPC a VPC ID\. You will need this for this for the next step\. To view your VPC ID, choose **Your VPCs** in the left pane of the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\.  
![\[VPC ID\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-id.png)

## Deploy to Elastic Beanstalk<a name="vpc-single-instance-create-env"></a>

After you set up your VPC, you can create your environment inside it and deploy your application to Elastic Beanstalk\. You can do this using the Elastic Beanstalk console, or you can use the AWS toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API\. If you use the Elastic Beanstalk console, you just need to upload your `.war` or `.zip` file and select the VPC settings inside the wizard\. Elastic Beanstalk then creates your environment inside your VPC and deploys your application\. Alternatively, you can use the AWS toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API to deploy your application\. To do this, you need to define your VPC option settings in a configuration file and deploy this file with your source bundle\. This topic provides instructions for both methods\.

### Deploying with the Elastic Beanstalk Console<a name="vpc-single-instance-new-console"></a>

When you create an Elastic Beanstalk application or launch an environment, the Elastic Beanstalk console walks you through creating your environment inside a VPC\. For more information, see [Managing and Configuring AWS Elastic Beanstalk Applications](applications.md)\.

You'll need to select the VPC ID and subnet ID for your instance\. By default, VPC creates a public subnet using 10\.0\.0\.0/24\. You can view your subnet IDs by choosing **Subnets** in the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\. 

![\[Subnet IDs for your VPC\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-subnets-pub.png)

### Deploying with the AWS Toolkits, Eb, CLI, or API<a name="vpc-single-instance-new-options"></a>

When deploying your application to Elastic Beanstalk using the AWS toolkits, EB CLI, AWS CLI, or API, you can specify your VPC option settings in a file and deploy it with your source bundle\. See [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md) for more information\.[aws:ec2:vpc](command-options-general.md#command-options-general-ec2vpc) Namespace:

VPCId  
The identifier of your VPC\.

Subnets  
The identifier\(s\) of the subnet\(s\) to launch the instances in\.   
You can specify multiple identifiers by separating them with a comma\.

```
option_settings:
  aws:ec2:vpc:
    VPCId: "vpd_id"
    Subnets: "instance_subnet, etc"
```