# Configuration<a name="troubleshooting-configuration"></a>

**Event:** *You cannot configure an Elastic Beanstalk environment with values for both the Elastic Load Balancing Target option and Application Healthcheck URL option*

The `Target` option in the `aws:elb:healthcheck` namespace is deprecated\. Remove the `Target` option namespace\) from your environment and try updating again\.

**Event:** *ELB cannot be attached to multiple subnets in the same AZ\.*

This message can be seen if you try to move a load balancer between subnets in the same Availability Zone\. Changing subnets on the load balancer requires moving it out of the original availability zone\(s\) and then back into the original with the desired subnets\. During the process, all of your instances will be migrated between AZs, causing significant downtime\. Instead, consider creating a new environment and [perform a CNAME swap](using-features.CNAMESwap.md)\.