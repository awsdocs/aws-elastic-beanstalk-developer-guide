# Elastic Beanstalk Platform Support Policy<a name="platforms-support-policy"></a>

AWS Elastic Beanstalk provides a variety of platforms for running applications on AWS\. Elastic Beanstalk supports platform versions that still receive ongoing minor and patch updates from their suppliers \(owners or community\)\. For a complete definition of related terms, see [Elastic Beanstalk Platforms Glossary](platforms-glossary.md)\.

When a component \(operating system \[OS\], runtime, application server, or web server\) of a supported platform version is marked End of Life \(EOL\) by its supplier, Elastic Beanstalk marks the platform version as retired\. When a platform version is marked as retired, Elastic Beanstalk no longer makes it available to both existing and new Elastic Beanstalk customers for deployments to new environments\. Retired platform versions are available to existing customer environments for a period of 90 days from the published retirement date\.

Elastic Beanstalk isn't able to provide security updates, technical support, or hotfixes for retired platform versions due to the supplier marking their component EOL\. For existing customers running an Elastic Beanstalk environment on a retired platform version beyond the 90 day period, Elastic Beanstalk may need to automatically remove the Elastic Beanstalk components and transfer ongoing management and support responsibility of the running application and associated AWS resources to the customer\. To continue to benefit from important security, performance, and functionality enhancements offered by component suppliers in more recent releases, we strongly encourage you to update all your Elastic Beanstalk environments to a supported platform version\.

## Retired Platform Version Schedule<a name="platforms-support-policy.depracation"></a>

The following tables list existing platform components that are either marked as retired or have retirement dates scheduled in the next 12 months\.


**Operating System \(OS\) versions**  

|  OS version  |  Availability end date  | 
| --- | --- | 
| Windows Server 2008 R2\[1\] | October 16, 2019 | 

\[1\] Previously, we published a retirement date of March 1, 2020 for Windows Server 2008 R2\. Due to changes in Microsoft's licensing policy we now have a scheduled retirement date of *October 16, 2019* for the platform\. If you have questions about your licensing or rights to Microsoft software, please consult your legal team, Microsoft, or your Microsoft reseller\.


**Web server versions**  

|  Web server version  |  Availability end date  | 
| --- | --- | 
| Apache HTTP Server 2\.2 | March 1, 2020 | 
| Nginx 1\.12\.2 | March 1, 2020 | 


**Runtime versions**  

|  Runtime version  |  Availability end date  | 
| --- | --- | 
| GlassFish 4\.x | March 1, 2020 | 
| Go 1\.3–1\.10 | March 1, 2020 | 
| Java 6 | March 1, 2020 | 
| Node\.js 4\.x–8\.x | March 1, 2020 | 
| PHP 5\.4–5\.6 | March 1, 2020 | 
| PHP 7\.0–7\.1 | March 1, 2020 | 
| Python 2\.6, 2\.7, 3\.4 | March 1, 2020 | 
| Ruby 1\.9\.3 | March 1, 2020 | 
| Ruby 2\.0–2\.3 | March 1, 2020 | 


**Application server versions**  

|  Application server version  |  Availability end date  | 
| --- | --- | 
| Tomcat 6 | March 1, 2020 | 
| Tomcat 8 | March 1, 2020 | 