# Example: ElastiCache<a name="customize-environment-resources-elasticache"></a>

The following samples add an Amazon ElastiCache cluster to EC2\-Classic and EC2\-VPC \(both default and custom [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\)\) platforms\. For more information about these platforms and how you can determine which ones EC2 supports for your region and your AWS account, see [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html)\. Then refer to the section in this topic that applies to your platform\.
+ [EC2\-classic platforms](#customize-environment-resources-elasticache-classic)
+ [EC2\-VPC \(default\)](#customize-environment-resources-elasticache-defaultvpc)
+ [EC2\-VPC \(custom\)](#customize-environment-resources-elasticache-targetedvpc)

## EC2\-classic platforms<a name="customize-environment-resources-elasticache-classic"></a>

This sample adds an Amazon ElastiCache cluster to an environment with instances launched into the EC2\-Classic platform\. All of the properties that are listed in this example are the minimum required properties that must be set for each resource type\. You can download the example at [ElastiCache example](https://elasticbeanstalk.s3.amazonaws.com/extensions/ElastiCache.config)\. 

**Note**  
This example creates AWS resources, which you might be charged for\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\.

To use this example, do the following:

1. Create an `[\.ebextensions](ebextensions.md)` directory in the top\-level directory of your source bundle\. 

1. Create two configuration files with the `.config` extension and place them in your `.ebextensions` directory\. One configuration file defines the resources, and the other configuration file defines the options\.

1. Deploy your application to Elastic Beanstalk\.

   YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Create a configuration file \(e\.g\., `elasticache.config`\) that defines the resources\. In this example, we create the ElastiCache cluster by specifying the name of the ElastiCache cluster resource \(`MyElastiCache`\), declaring its type, and then configuring the properties for the cluster\. The example references the name of the ElastiCache security group resource that gets created and defined in this configuration file\. Next, we create an ElastiCache security group\. We define the name for this resource, declare its type, and add a description for the security group\. Finally, we set the ingress rules for the ElastiCache security group to allow access only from instances inside the ElastiCache security group \(`MyCacheSecurityGroup`\) and the Elastic Beanstalk security group \(`AWSEBSecurityGroup`\)\. The parameter name, `AWSEBSecurityGroup`, is a fixed resource name provided by Elastic Beanstalk\. You must add `AWSEBSecurityGroup` to your ElastiCache security group ingress rules in order for your Elastic Beanstalk application to connect to the instances in your ElastiCache cluster\. 

```
#This sample requires you to create a separate configuration file that defines the custom option settings for CacheCluster properties.
          
Resources:
  MyElastiCache:
    Type: AWS::ElastiCache::CacheCluster
    Properties:
      CacheNodeType: 
         Fn::GetOptionSetting:
             OptionName : CacheNodeType
             DefaultValue: cache.m1.small
      NumCacheNodes: 
           Fn::GetOptionSetting:
             OptionName : NumCacheNodes
             DefaultValue: 1
      Engine: 
           Fn::GetOptionSetting:
             OptionName : Engine
             DefaultValue: memcached
      CacheSecurityGroupNames:
        - Ref: MyCacheSecurityGroup
  MyCacheSecurityGroup:
    Type: AWS::ElastiCache::SecurityGroup
    Properties:
      Description: "Lock cache down to webserver access only"
  MyCacheSecurityGroupIngress:
    Type: AWS::ElastiCache::SecurityGroupIngress
    Properties:
      CacheSecurityGroupName: 
        Ref: MyCacheSecurityGroup
      EC2SecurityGroupName:
        Ref: AWSEBSecurityGroup
```

For more information about the resources used in this example configuration file, see the following references: 
+ [AWS::ElastiCache::CacheCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-cache-cluster.html)
+ [AWS::ElastiCache::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-security-group.html)
+ [AWS::ElastiCache:SecurityGroupIngress](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-security-group-ingress.html)

Create a separate configuration file called `options.config` and define the custom option settings\. 

```
option_settings:
  "aws:elasticbeanstalk:customoption":
     CacheNodeType : cache.m1.small
     NumCacheNodes : 1
     Engine : memcached
```

These lines tell Elastic Beanstalk to get the values for the **CacheNodeType, NumCacheNodes, and Engine** properties from the **CacheNodeType, NumCacheNodes, and Engine** values in a config file \(options\.config in our example\) that contains an option\_settings section with an **aws:elasticbeanstalk:customoption** section that contains a name\-value pair that contains the actual value to use\. In the example above, this means cache\.m1\.small, 1, and memcached would be used for the values\. For more information about `Fn::GetOptionSetting`, see [Functions](ebextensions-functions.md)\.

## EC2\-VPC \(default\)<a name="customize-environment-resources-elasticache-defaultvpc"></a>

This sample adds an Amazon ElastiCache cluster to an environment with instances launched into the EC2\-VPC platform\. Specifically, the information in this section applies to a scenario where EC2 launches instances into the default VPC\. All of the properties in this example are the minimum required properties that must be set for each resource type\. For more information about default VPCs, see [Your Default VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)\.

**Note**  
This example creates AWS resources, which you might be charged for\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\.

To use this example, do the following:

1. Create an `[\.ebextensions](ebextensions.md)` directory in the top\-level directory of your source bundle\. 

1. Create two configuration files with the `.config` extension and place them in your `.ebextensions` directory\. One configuration file defines the resources, and the other configuration file defines the options\.

1. Deploy your application to Elastic Beanstalk\.

   YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Now name the resources configuration file `elasticache.config`\. To create the ElastiCache cluster, this example specifies the name of the ElastiCache cluster resource \(`MyElastiCache`\), declares its type, and then configures the properties for the cluster\. The example references the ID of the security group resource that we create and define in this configuration file\.

Next, we create an EC2 security group\. We define the name for this resource, declare its type, add a description, and set the ingress rules for the security group to allow access only from instances inside the Elastic Beanstalk security group \(`AWSEBSecurityGroup`\)\. \(The parameter name, `AWSEBSecurityGroup`, is a fixed resource name provided by Elastic Beanstalk\. You must add `AWSEBSecurityGroup` to your ElastiCache security group ingress rules in order for your Elastic Beanstalk application to connect to the instances in your ElastiCache cluster\.\)

The ingress rules for the EC2 security group also define the IP protocol and port numbers on which the cache nodes can accept connections\. For Redis, the default port number is `6379`\.

```
#This sample requires you to create a separate configuration file that defines the custom option settings for CacheCluster properties.

Resources:
  MyCacheSecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: "Lock cache down to webserver access only"
      SecurityGroupIngress :
        - IpProtocol : "tcp"
          FromPort :
            Fn::GetOptionSetting:
              OptionName : "CachePort"
              DefaultValue: "6379"
          ToPort :
            Fn::GetOptionSetting:
              OptionName : "CachePort"
              DefaultValue: "6379"
          SourceSecurityGroupName:
            Ref: "AWSEBSecurityGroup"
  MyElastiCache:
    Type: "AWS::ElastiCache::CacheCluster"
    Properties:
      CacheNodeType:
        Fn::GetOptionSetting:
          OptionName : "CacheNodeType"
          DefaultValue : "cache.t2.micro"
      NumCacheNodes:
        Fn::GetOptionSetting:
          OptionName : "NumCacheNodes"
          DefaultValue : "1"
      Engine:
        Fn::GetOptionSetting:
          OptionName : "Engine"
          DefaultValue : "redis"
      VpcSecurityGroupIds:
        -
          Fn::GetAtt:
            - MyCacheSecurityGroup
            - GroupId

Outputs:
  ElastiCache:
    Description : "ID of ElastiCache Cache Cluster with Redis Engine"
    Value :
      Ref : "MyElastiCache"
```

For more information about the resources used in this example configuration file, see the following references: 
+ [AWS::ElastiCache::CacheCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-cache-cluster.html)
+ [AWS::EC2::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)

Next, name the options configuration file `options.config` and define the custom option settings\. 

```
option_settings:
  "aws:elasticbeanstalk:customoption":
    CacheNodeType : cache.t2.micro
    NumCacheNodes : 1
    Engine : redis
    CachePort : 6379
```

These lines tell Elastic Beanstalk to get the values for the `CacheNodeType`, `NumCacheNodes`, `Engine`, and `CachePort` properties from the `CacheNodeType`, `NumCacheNodes`, `Engine`, and `CachePort` values in a config file \(`options.config` in our example\)\. That file includes an `aws:elasticbeanstalk:customoption` section \(under `option_settings`\) that contains name\-value pairs with the actual values to use\. In the preceding example, `cache.t2.micro`, `1`, `redis`, and `6379` would be used for the values\. For more information about `Fn::GetOptionSetting`, see [Functions](ebextensions-functions.md)\.

## EC2\-VPC \(custom\)<a name="customize-environment-resources-elasticache-targetedvpc"></a>

If you create a custom VPC on the EC2\-VPC platform and specify it as the VPC into which EC2 launches instances, the process of adding an Amazon ElastiCache cluster to your environment differs from that of a default VPC\. The main difference is that you must create a subnet group for the ElastiCache cluster\. All of the properties in this example are the minimum required properties that must be set for each resource type\.

**Note**  
This example creates AWS resources, which you might be charged for\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\.

To use this example, do the following:

1. Create an `[\.ebextensions](ebextensions.md)` directory in the top\-level directory of your source bundle\. 

1. Create two configuration files with the `.config` extension and place them in your `.ebextensions` directory\. One configuration file defines the resources, and the other configuration file defines the options\.

1. Deploy your application to Elastic Beanstalk\.

   YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Now name the resources configuration file `elasticache.config`\. To create the ElastiCache cluster, this example specifies the name of the ElastiCache cluster resource \(`MyElastiCache`\), declares its type, and then configures the properties for the cluster\. The properties in the example reference the name of the subnet group for the ElastiCache cluster as well as the ID of security group resource that we create and define in this configuration file\.

Next, we create an EC2 security group\. We define the name for this resource, declare its type, add a description, the VPC ID, and set the ingress rules for the security group to allow access only from instances inside the Elastic Beanstalk security group \(`AWSEBSecurityGroup`\)\. \(The parameter name, `AWSEBSecurityGroup`, is a fixed resource name provided by Elastic Beanstalk\. You must add `AWSEBSecurityGroup` to your ElastiCache security group ingress rules in order for your Elastic Beanstalk application to connect to the instances in your ElastiCache cluster\.\)

The ingress rules for the EC2 security group also define the IP protocol and port numbers on which the cache nodes can accept connections\. For Redis, the default port number is `6379`\. Finally, this example creates a subnet group for the ElastiCache cluster\. We define the name for this resource, declare its type, and add a description and ID of the subnet in the subnet group\.

**Note**  
We recommend that you use private subnets for the ElastiCache cluster\. For more information about a VPC with a private subnet, see [https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html)\.

```
#This sample requires you to create a separate configuration file that defines the custom option settings for CacheCluster properties.

Resources:
  MyElastiCache:
    Type: "AWS::ElastiCache::CacheCluster"
    Properties:
      CacheNodeType:
        Fn::GetOptionSetting:
          OptionName : "CacheNodeType"
          DefaultValue : "cache.t2.micro"
      NumCacheNodes:
        Fn::GetOptionSetting:
          OptionName : "NumCacheNodes"
          DefaultValue : "1"
      Engine:
        Fn::GetOptionSetting:
          OptionName : "Engine"
          DefaultValue : "redis"
      CacheSubnetGroupName:
        Ref: "MyCacheSubnets"
      VpcSecurityGroupIds:
        - Ref: "MyCacheSecurityGroup"
  MyCacheSecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: "Lock cache down to webserver access only"
      VpcId:
        Fn::GetOptionSetting:
          OptionName : "VpcId"
      SecurityGroupIngress :
        - IpProtocol : "tcp"
          FromPort :
            Fn::GetOptionSetting:
              OptionName : "CachePort"
              DefaultValue: "6379"
          ToPort :
            Fn::GetOptionSetting:
              OptionName : "CachePort"
              DefaultValue: "6379"
          SourceSecurityGroupId:
            Ref: "AWSEBSecurityGroup"
  MyCacheSubnets:
    Type: "AWS::ElastiCache::SubnetGroup"
    Properties:
      Description: "Subnets for ElastiCache"
      SubnetIds:
        Fn::GetOptionSetting:
          OptionName : "CacheSubnets"
Outputs:
  ElastiCache:
    Description : "ID of ElastiCache Cache Cluster with Redis Engine"
    Value :
      Ref : "MyElastiCache"
```

For more information about the resources used in this example configuration file, see the following references: 
+ [AWS::ElastiCache::CacheCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-cache-cluster.html)
+ [AWS::EC2::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)
+ [AWS::ElastiCache::SubnetGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-subnetgroup.html)

Next, name the options configuration file `options.config` and define the custom option settings\.

**Note**  
In the following example, replace the example `CacheSubnets` and `VpcId` values with your own subnets and VPC\.

```
option_settings:
  "aws:elasticbeanstalk:customoption":
    CacheNodeType : cache.t2.micro
    NumCacheNodes : 1
    Engine : redis
    CachePort : 6379
    CacheSubnets:
      - subnet-1a1a1a1a
      - subnet-2b2b2b2b
      - subnet-3c3c3c3c
    VpcId: vpc-4d4d4d4d
```

These lines tell Elastic Beanstalk to get the values for the `CacheNodeType`, `NumCacheNodes`, `Engine`, `CachePort`, `CacheSubnets`, and `VpcId` properties from the `CacheNodeType`, `NumCacheNodes`, `Engine`, `CachePort`, `CacheSubnets`, and `VpcId` values in a config file \(`options.config` in our example\)\. That file includes an `aws:elasticbeanstalk:customoption` section \(under `option_settings`\) that contains name\-value pairs with sample values\. In the example above, `cache.t2.micro`, `1`, `redis`, `6379`, `subnet-1a1a1a1a`, `subnet-2b2b2b2b`, `subnet-3c3c3c3c`, and `vpc-4d4d4d4d` would be used for the values\. For more information about `Fn::GetOptionSetting`, see [Functions](ebextensions-functions.md)\.