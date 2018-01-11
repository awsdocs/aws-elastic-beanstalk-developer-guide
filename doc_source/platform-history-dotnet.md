# \.NET on Windows Server with IIS Platform History<a name="platform-history-dotnet"></a>

This page lists the previous versions of AWS Elastic Beanstalk's \.NET platforms and the dates that each version was current\. Platform versions that you used to launch or update an environment in the last 30 days remain available \(to the using account, in the used region\) even after they are no longer current\.

See the Supported Platforms page for information on the latest version of each platform supported by Elastic Beanstalk\. Detailed release notes are available for recent releases at [aws\.amazon\.com/releasenotes](https://aws.amazon.com/releasenotes/AWS-Elastic-Beanstalk)\. 

## November 20, 2017 – December 18, 2017<a name="platform-history-2017-11-20"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 10\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server Core 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 10\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   |   *64bit Windows Server 2012 running IIS 8*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 7\.5  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.10\.13  |  3\.15\.172\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.10\.13  |  3\.15\.172\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |  2017\.10\.13  |  3\.15\.172\.0  |  4\.9\.2188\.0  |  2\.2\.30\.0  |  3\.6  |  1\.0\.0  | 

## August 28, 2017 – November 19, 2017<a name="platform-history-2017-08-28"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 10\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server Core 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 10\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  \.NET Core 2\.0, supports 1\.1\.x, 1\.0\.x \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   |   *64bit Windows Server 2012 running IIS 8*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  \.NET Framework 4\.7, supports 4\.x, 2\.0, 1\.x  |  IIS 7\.5  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.08\.09  |  3\.3\.103\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.08\.09  |  3\.3\.103\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |  2017\.08\.09  |  3\.3\.58\.0  |  4\.9\.2016  |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |  2017\.08\.09  |  3\.3\.58\.0  |  4\.9\.2016  |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |  2017\.08\.09  |  3\.3\.102\.0  |  4\.9\.2016  |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |  2017\.08\.09  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |  2017\.08\.09  |  3\.3\.58\.0  |  4\.9\.2016  |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |  2017\.08\.09  |  3\.3\.58\.0  |  4\.9\.2016  |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8**   |  2017\.08\.09  |  3\.3\.102\.0  |  4\.9\.2016  |  2\.0\.879\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |  2017\.08\.09  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 

## July 24, 2017 – Aug 27, 2017<a name="platform-history-27"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 10\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server Core 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 10\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   |   *64bit Windows Server 2012 running IIS 8*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.07\.13  |  3\.3\.103\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.07\.13  |  3\.3\.103\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |  2017\.07\.13  |  3\.3\.102\.0  |  4\.9\.1981  |  2\.0\.847\.0  |  3\.6  |  1\.0\.0  | 

## July 17, 2017 – July 23, 2017<a name="platform-history-26"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 10\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server Core 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 10\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   |   *64bit Windows Server 2012 running IIS 8*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.06\.14  |  3\.3\.103\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.06\.14  |  3\.3\.103\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.682\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |  2017\.06\.14  |  3\.3\.102\.0  |  4\.9\.1900\.0  |  2\.0\.805\.0  |  3\.6  |  1\.0\.0  | 

## June 26, 2017 – July 16, 2017<a name="platform-history-25"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 10\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |   *64bit Windows Server Core 2016 v1\.2\.0 running IIS 10\.0*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 10\.0  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.05\.10  |  3\.14\.61\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.767\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server Core 2016 with IIS 10\.0 version 1\.2\.0**   |  2017\.05\.10  |  3\.14\.61\.0  |   * [SSM only](http://docs.aws.amazon.com/systems-manager/latest/userguide/) *   |  2\.0\.767\.0  |  3\.6  |  1\.0\.0  | 

## May 16, 2017 – July 16, 2017<a name="platform-history-24"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:

### Configuration basics<a name="concepts.platforms.net.basics"></a>


****  

|  Configuration  |  Solution Stack Name  |  Framework  |  Proxy Server  | 
| --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.2, 1\.0\.5  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   |   *64bit Windows Server 2012 running IIS 8*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  \.NET v4\.7, supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

### More details<a name="concepts.platforms.net.details"></a>


****  

|  Configuration  |  AMI version  |  AWS SDK for \.NET  |  EC2Config  |  SSM Agent  |  Web Deploy  |  AWS X‑Ray  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.761\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.761\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.682\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.761\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.761\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.761\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2012 with IIS 8**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.682\.0  |  3\.6  |  1\.0\.0  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   |  2017\.04\.12  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  2\.0\.761\.0  |  3\.6  |  1\.0\.0  | 

## May 4, 2017 – May 15, 2017<a name="platform-history-23"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  AWS X\-Ray  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2017\.04\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.14\.61\.0  |  4\.9\.1775\.0  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 

## April 4, 2017 – May 3, 2017<a name="platform-history-22"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  AWS X\-Ray  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.1, 1\.0\.4  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2017\.03\.15  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.13\.767\.0  |  4\.7\.1631  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 

## January 16, 2017 – Apr 3, 2017<a name="platform-history-21"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  AWS X\-Ray  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0, 1\.03  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0, 1\.03  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0, 1\.03  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0, 1\.03  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.12\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.621\.0  |  4\.1\.1396\.0  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for January 2017](https://technet.microsoft.com/en-us/library/security/ms17-Jan.aspx) 

## December 18, 2016 – January 15, 2017<a name="platform-history-20"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  AWS X\-Ray  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.11\.09  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.560\.0  |  3\.19\.1153\.0  |  3\.6  |  1\.0\.0  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for December 2016](https://technet.microsoft.com/en-us/library/security/ms16-Dec.aspx) 

## November 16, 2016 – December 18, 2016<a name="platform-history-19"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.10\.12  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.520\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for November 2016](https://technet.microsoft.com/en-us/library/security/ms16-Nov) 

## October 21, 2016 – November 16, 2016<a name="platform-history-18"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for October 2016](https://technet.microsoft.com/en-us/library/security/ms16-Oct.aspx) 

## September 26, 2016 – October 21, 2016<a name="platform-history-17"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0\.1  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.09\.14  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.459\.0  |  3\.19\.1153\.0  |  3\.6  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for September 2016](https://technet.microsoft.com/en-us/library/security/ms16-Sep.aspx) 

## August 23, 2016 – September 26, 2016<a name="platform-history-16"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.2\.0**   *64bit Windows Server Core 2012 R2 v1\.2\.0 running IIS 8\.5*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.2\.0**   *64bit Windows Server 2012 v1\.2\.0 running IIS 8*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.2\.0**   *64bit Windows Server 2008 R2 v1\.2\.0 running IIS 7\.5*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0 ASP\.NET Core v1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.07\.26  |  \.NET v4\.6\.2, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.406\.0  |  3\.18\.1118  |  3\.6  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for July 2016](https://technet.microsoft.com/en-us/library/security/ms16-jul.aspx), [Microsoft Security Bulletin Summary for August 2016](https://technet.microsoft.com/en-us/library/security/ms16-aug.aspx) 

## June 21, 2016 – August 23, 2016<a name="platform-history-15"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.1\.0**   *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.1\.0**   *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.1\.0**   *64bit Windows Server 2012 v1\.1\.0 running IIS 8*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.1\.0**   *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for June 2016](https://technet.microsoft.com/en-us/library/security/ms16-jun.aspx) 

## May 25, 2016 – June 21, 2016<a name="platform-history-14"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*   |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|   **Windows Server 2012 R2 with IIS 8\.5 version 1\.1\.0**   *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5 version 1\.1\.0**   *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8 version 1\.1\.0**   *64bit Windows Server 2012 v1\.1\.0 running IIS 8*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5 version 1\.1\.0**   *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 7\.5  | 
|   **Windows Server 2012 R2 with IIS 8\.5**   *64bit Windows Server 2012 R2 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 R2 Server Core with IIS 8\.5**   *64bit Windows Server Core 2012 R2 running IIS 8\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8\.5  | 
|   **Windows Server 2012 with IIS 8**   *64bit Windows Server 2012 running IIS 8*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 8  | 
|   **Windows Server 2008 R2 with IIS 7\.5**   *64bit Windows Server 2008 R2 running IIS 7\.5*   |  2016\.05\.11  |  \.NET v4\.6\.1, Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.9\.329\.0  |  3\.15\.880  |  3\.6  |  IIS 7\.5  | 

 1 [Microsoft Security Bulletin Summary for May 2016](https://technet.microsoft.com/en-us/library/security/ms16-may.aspx) 

## April 25, 2016 – May 25, 2016<a name="platform-history-13"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*  |  AMI version  |  Framework  |  AWS SDK for \.NET  |  EC2Config  |  WebDeploy  |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5 version 1\.1\.0** *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5 version 1\.1\.0** *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8 version 1\.1\.0** *64bit Windows Server 2012 v1\.1\.0 running IIS 8*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5 version 1\.1\.0** *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 7\.5  | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  2016\.03\.09  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  3\.8\.306\.0  |  3\.14\.786  |  3\.6  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for April 2016](https://technet.microsoft.com/en-us/library/security/ms16-apr.aspx)

## March 23, 2016 – April 25, 2016<a name="platform-history-12"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*  | AMI version |  Framework  | AWS SDK for \.NET | EC2Config |  Proxy Server  | 
| --- | --- | --- | --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5 version 1\.1\.0** *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5 version 1\.1\.0** *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8 version 1\.1\.0** *64bit Windows Server 2012 v1\.1\.0 running IIS 8*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5 version 1\.1\.0** *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 7\.5  | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  2016\.02\.10  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for March 2016](https://technet.microsoft.com/en-us/library/security/ms16-Mar)

## February 29, 2016 – March 23, 2016<a name="platform-history-11"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*  | AMI version |  Framework  | AWS SDK for \.NET | EC2Config |  Web Server  | 
| --- | --- | --- | --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5 version 1\.1\.0** *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5 version 1\.1\.0** *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8 version 1\.1\.0** *64bit Windows Server 2012 v1\.1\.0 running IIS 8*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5 version 1\.1\.0** *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 7\.5  | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  2016\.01\.25  |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  v3\.1\.36\.1  |  3\.12\.649  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for February 2016](https://technet.microsoft.com/en-us/library/security/ms16-Feb)

## January 28, 2016 – February 29, 2016<a name="platform-history-10"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*  | AMI version |  Framework  | AWS SDK for \.NET |  Web Server  | 
| --- | --- | --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5 version 1\.1\.0** *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5 version 1\.1\.0** *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8 version 1\.1\.0** *64bit Windows Server 2012 v1\.1\.0 running IIS 8*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5 version 1\.1\.0** *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 7\.5  | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  | 2015\.12\.31 |  \.NET v4\.6\.1 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  | v3\.1\.36\.1 |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for January 2016](https://technet.microsoft.com/en-us/library/security/ms16-Jan)

## December 15, 2015 – January 28, 2016<a name="platform-history-09"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*  |  Framework  |  Web Server  | 
| --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5 version 1\.1\.0** *64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5 version 1\.1\.0** *64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8 version 1\.1\.0** *64bit Windows Server 2012 v1\.1\.0 running IIS 8*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5 version 1\.1\.0** *64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5*  |  \.NET v4\.5 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  \.NET v4\.5 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for November 2015](https://technet.microsoft.com/en-us/library/security/ms15-nov.aspx), [Microsoft Security Bulletin Summary for December 2015](https://technet.microsoft.com/en-us/library/security/ms15-dec.aspx)

## October 21, 2015 – December 15, 2015<a name="platform-history-08"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  Configuration and *Solution Stack Name*  |  Framework  |  Web Server  | 
| --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5 version 1\.0\.0** *64bit Windows Server 2012 R2 v1\.0\.0 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5 version 1\.0\.0** *64bit Windows Server Core 2012 R2 v1\.0\.0 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8 version 1\.0\.0** *64bit Windows Server 2012 v1\.0\.0 running IIS 8*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5 version 1\.0\.0** *64bit Windows Server 2008 R2 v1\.0\.0 running IIS 7\.5*  |  \.NET v4\.5 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  \.NET v4\.6 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  \.NET v4\.5 Supports runtimes 4, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

## September 14, 2015 – October 21, 2015<a name="platform-history-07"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

| Configuration and *Solution Stack Name* | Framework | Web Server | 
| --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for September 2015](https://technet.microsoft.com/en-us/library/security/ms15-sep.aspx)

## August 20, 2015 – September 14, 2015<a name="platform-history-06"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

| Configuration and *Solution Stack Name* | Framework | Web Server | 
| --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for August 2015](https://technet.microsoft.com/en-us/library/security/ms15-aug.aspx)

## July 21, 2015 – August 20, 2015<a name="platform-history-05"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

| Configuration and *Solution Stack Name* | Framework | Web Server | 
| --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for July 2015](https://technet.microsoft.com/en-us/library/security/ms15-jul.aspx)

## June 12, 2015 – July 21, 2015<a name="platform-history-04"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

| Configuration and *Solution Stack Name* | Framework | Web Server | 
| --- | --- | --- | 
|  **Windows Server 2012 R21 with IIS 8\.5** *64bit Windows Server 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 2012 R21 Server Core with IIS 8\.5** *64bit Windows Server Core 2012 R2 running IIS 8\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  **Windows Server 20121 with IIS 8** *64bit Windows Server 2012 running IIS 8*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  **Windows Server 2008 R21 with IIS 7\.5** *64bit Windows Server 2008 R2 running IIS 7\.5*  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for June 2015](https://technet.microsoft.com/en-us/library/security/ms15-jun.aspx)

## April 16, 2015 – June 12, 2015<a name="platform-history-03"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  **IIS Configurations**  | 
| --- | 
| Name | AMI | Language | Web Server | 
|  64bit Windows Server 2012 R21 running IIS 8\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  64bit Windows Server Core 2012 R21 running IIS 8\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  64bit Windows Server 20121 running IIS 8  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  64bit Windows Server 2008 R21 running IIS 7\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin Summary for May 2015](https://technet.microsoft.com/en-us/library/security/ms15-may.aspx)

## August 6, 2014 – April 16, 2015<a name="platform-history-02"></a>

The following Elastic Beanstalk platform configurations for \.NET were current during this date range:


****  

|  **IIS Configurations**  | 
| --- | 
| Name | AMI | Language | Web Server | 
|  64bit Windows Server 2012 R21 running IIS 8\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  64bit Windows Server Core 2012 R21 running IIS 8\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8\.5  | 
|  64bit Windows Server 20121 running IIS 8  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  64bit Windows Server 2008 R21 running IIS 7\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin MS14\-066 \- Critical](https://technet.microsoft.com/library/security/ms14-066)

## Prior to August 6, 2014<a name="platform-history-01"></a>

The following Elastic Beanstalk platform configurations for \.NET were current prior to August 6, 2014:


****  

|  **IIS Configurations**  | 
| --- | 
| Name | AMI | Language | Web Server | 
|  64bit Windows Server 20121 running IIS 8  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 8  | 
|  64bit Windows Server 2008 R21 running IIS 7\.5  |  Custom  |  \.NET v4\.5 Also supports 4\.0, 3\.5, 3\.0, 2\.0, 1\.1 and 1\.0  |  IIS 7\.5  | 

1[Microsoft Security Bulletin MS14\-066 \- Critical](https://technet.microsoft.com/library/security/ms14-066)