# eb ssh<a name="eb3-ssh"></a>

## Description<a name="eb3-sshdescription"></a>

**Note**  
This command does not work with environments running Windows Server instances\.

Connect to a Linux Amazon EC2 instance in your environment using Secure Shell \(SSH\)\. If an environment has multiple running instances, EB CLI prompts you to specify which instance you want to connect to\. To use this command, SSH must be installed on your local machine and available from the command line\. Private key files must be located in a folder named `.ssh` under your user directory, and the EC2 instances in your environment must have public IP addresses\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command also connects to instances in the custom environment\.

**SSH keys**  
If you have not previously configured SSH, you can use the EB CLI to create a key when running eb init\. If you have already run eb init, run it again with the `--interactive` option and select **Yes** and **Create New Keypair** when prompted to set up SSH\. Keys created during this process will be stored in the proper folder by the EB CLI\.

This command temporarily opens port 22 in your environment's security group for incoming traffic from 0\.0\.0\.0/0 \(all IP addresses\) if no rules for port 22 are already in place\. If you have configured your environment's security group to open port 22 to a restricted CIDR range for increased security, the EB CLI will respect that setting and forgo any changes to the security group\. To override this behavior and force the EB CLI to open port 22 to all incoming traffic, use the `--force` option\.

See [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups) for information on configuring your environment's security group\.

## Syntax<a name="eb3-sshsyntax"></a>

 eb ssh 

 eb ssh *environment\-name* 

## Options<a name="eb3-sshoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-i` or `--instance`  |  Specifies the instance ID of the instance to which you connect\. We recommend that you use this option\.  | 
|  `-n` or `--number`  |  Specify the instance to connect to by number\.  | 
|  `-o` or `--keep_open`  |  Leave port 22 open on the security group after the SSH session ends\.  | 
|  `--command`  |  Execute a shell command on the specified instance instead of starting an SSH session\.  | 
|  `--custom`  |  Specify an SSH command to use instead of 'ssh \-i keyfile'\. Do not include the remote user and hostname\.  | 
|  `--setup`  |  Change the key pair assigned to the environment's instances \(requires instances to be replaced\)\.  | 
|  `--force`  |  Open port 22 to incoming traffic from 0\.0\.0\.0/0 in the environment's security group, even if the security group is already configured for SSH\. Use this option if your environment's security group is configured to open port 22 to a restricted CIDR range that does not include the IP address that you are trying to connect from\.  | 
|  `--timeout` *minutes*  |  Set number of minutes before the command times out\. Can only be used with the `--setup` argument\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-sshoutput"></a>

If successful, the command opens an SSH connection to the instance\.

## Example<a name="eb3-sshexample"></a>

The following example connects you to the specified environment\.

```
$ eb ssh
Select an instance to ssh into
1) i-96133799
2) i-5931e053
(default is 1): 1
INFO: Attempting to open port 22.
INFO: SSH port 22 open.
The authenticity of host '54.191.45.125 (54.191.45.125)' can't be established.
RSA key fingerprint is ee:69:62:df:90:f7:63:af:52:7c:80:60:1b:3b:51:a9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '54.191.45.125' (RSA) to the list of known hosts.

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2014.09-release-notes/
No packages needed for security; 1 packages available
Run "sudo yum update" to apply all updates.
[ec2-user@ip-172-31-8-185 ~]$ ls
[ec2-user@ip-172-31-8-185 ~]$ exit
logout
Connection to 54.191.45.125 closed.
INFO: Closed port 22 on ec2 instance security group
```