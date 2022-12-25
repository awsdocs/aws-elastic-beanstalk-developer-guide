# Elastic Beanstalk platform support policy<a name="platforms-support-policy"></a>

AWS Elastic Beanstalk provides a variety of platforms for running applications on AWS\. Elastic Beanstalk supports platform branches that still receive ongoing minor and patch updates from their suppliers \(owners or community\)\. For a complete definition of related terms, see [Elastic Beanstalk platforms glossary](platforms-glossary.md)\.

When a component \(operating system \[OS\], runtime, application server, or web server\) of a supported platform branch is marked End of Life \(EOL\) by its supplier, Elastic Beanstalk marks the platform branch as retired\. When a platform branch is marked as retired, Elastic Beanstalk no longer makes it available to both existing and new Elastic Beanstalk customers for deployments to new environments\. Retired platform branches are available to existing customer environments for a period of 90 days from the published retirement date\.

Elastic Beanstalk isn't able to provide security updates, technical support, or hotfixes for retired platform branches due to the supplier marking their component EOL\. For existing customers running an Elastic Beanstalk environment on a retired platform version beyond the 90 day period, Elastic Beanstalk may need to automatically remove the Elastic Beanstalk components and transfer ongoing management and support responsibility of the running application and associated AWS resources to the customer\. To continue to benefit from important security, performance, and functionality enhancements offered by component suppliers in more recent releases, we strongly encourage you to update all your Elastic Beanstalk environments to a supported platform version\.

## Retiring platform branch schedule<a name="platforms-support-policy.depracation"></a>

The following tables list existing platform components that are either marked as retired or have retirement dates scheduled in the next 12 months\. The tables provide the availability end date for Elastic Beanstalk platform branches that contain these components\.

For a list of related Elastic Beanstalk retiring platform branches, see [platform versions scheduled for retirement](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-retiring.html) in the *AWS Elastic Beanstalk Platforms* guide\.


**Operating System \(OS\) versions**  

|  OS version  |  Availability end date  | 
| --- | --- | 
| Amazon Linux AMI \(AL1\) | June 30, 2022 | 
| Windows Server 2012 R1 | May 31, 2022 | 


**Runtime versions**  

|  Runtime version  |  Availability end date  | 
| --- | --- | 
| Corretto 8 with Tomcat 7 | April 30, 2022 | 
| Corretto 11 with Tomcat 7 | April 30, 2022 | 
| Preconfigured Docker \- GlassFish 5\.0 with Java 8 | June 30, 2022 | 
| Java 7 SE | June 30, 2022 | 
| Java 7 with Tomcat 7 | June 30, 2022 | 
| Node\.js 10\.x | April 30, 2022 \(Amazon Linux 2\) / June 30,2022 \(Amazon Linux AMI\) | 
| PHP 7\.2–7\.3 | April 30, 2022 \(Amazon Linux 2\) / June 30,2022 \(Amazon Linux AMI\) | 
| Python 3\.6 | June 30, 2022 | 
| Ruby 2\.4, Ruby 2\.6 | June 30, 2022 | 
| Ruby 2\.5 | April 30, 2022 \(Amazon Linux 2\) / June 30,2022 \(Amazon Linux AMI\) | 


**Application server versions**  

|  Application server version  |  Availability end date  | 
| --- | --- | 
| Tomcat 7 | April 30, 2022 \(Amazon Linux 2\) / June 30,2022 \(Amazon Linux AMI\) | 

## Retired platform branches<a name="platforms-support-policy.retired"></a>

The following tables list platform components that were marked as retired in the past\. The tables provide the date on which Elastic Beanstalk retired platform branches that contained these components\.


**Operating System \(OS\) versions**  

|  OS version  |  Platform retirement date  | 
| --- | --- | 
| Windows Server 2008 R2 | October 28, 2019 | 


**Web server versions**  

|  Web server version  |  Availability end date  | 
| --- | --- | 
| Apache HTTP Server 2\.2 | October 31, 2020 | 
| Nginx 1\.12\.2 | October 31, 2020 | 


**Runtime versions**  

|  Runtime version  |  Availability end date  | 
| --- | --- | 
| Go 1\.3–1\.10 | October 31, 2020 | 
| Java 6 | October 31, 2020 | 
| Node\.js 4\.x–8\.x | October 31, 2020 | 
| PHP 5\.4–5\.6 | October 31, 2020 | 
| PHP 7\.0–7\.1 | October 31, 2020 | 
| Python 2\.6, 2\.7, 3\.4 | October 31, 2020 | 
| Ruby 1\.9\.3 | October 31, 2020 | 
| Ruby 2\.0–2\.3 | October 31, 2020 | 


**Application server versions**  

|  Application server version  |  Availability end date  | 
| --- | --- | 
| Tomcat 6 | October 31, 2020 | 
| Tomcat 8 | October 31, 2020 | 