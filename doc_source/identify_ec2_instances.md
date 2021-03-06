# Identify EC2 Linux instances<a name="identify_ec2_instances"></a>

Your application might need to determine whether it is running on an EC2 instance\.

For information about identifying Windows instances, see [Identify EC2 Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/identify_ec2_instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Inspect the instance identity document<a name="inspect-document"></a>

For a definitive and cryptographically verified method of identifying an EC2 instance, check the instance identity document, including its signature\. These documents are available on every EC2 instance at the local, non\-routable address `http://169.254.169.254/latest/dynamic/instance-identity/`\. For more information, see [Instance identity documents](instance-identity-documents.md)\.

## Inspect the system UUID<a name="inspect-uuid"></a>

You can get the system UUID and look for the presence of the characters "ec2" or "EC2" in the beginning octet of the UUID\. This method to determine whether a system is an EC2 instance is quick but potentially inaccurate because there is a small chance that a system that is not an EC2 instance could have a UUID that starts with these characters\. Furthermore, for EC2 instances that are not using Amazon Linux, the distribution's implementation of SMBIOS might represent the UUID in little\-endian format, therefore the "EC2" characters do not appear at the beginning of the UUID\.

**Example : Get the UUID from the hypervisor**  
If `/sys/hypervisor/uuid` exists, you can use the following command:  

```
[ec2-user ~]$ cat /sys/hypervisor/uuid
```
In the following example output, the UUID starts with "ec2", which indicates that the system is probably an EC2 instance\.  

```
ec2e1916-9099-7caf-fd21-012345abcdef
```

**Example : Get the UUID from DMI \(HVM instances only\)**  
On HVM instances only, you can use the Desktop Management Interface \(DMI\)\.   
You can use the `dmidecode` tool to return the UUID\. On Amazon Linux, use the following command to install the `dmidecode` tool if it's not already installed on your instance:  

```
[ec2-user ~]$ sudo yum install dmidecode -y
```
Then run the following command:  

```
[ec2-user ~]$ sudo dmidecode --string system-uuid
```
Alternatively, use the following command:  

```
[ec2-user ~]$ sudo cat /sys/devices/virtual/dmi/id/product_uuid
```
In the following example output, the UUID starts with "EC2", which indicates that the system is probably an EC2 instance\.  

```
EC2E1916-9099-7CAF-FD21-01234ABCDEF
```
In the following example output, the UUID is represented in little\-endian format\.  

```
45E12AEC-DCD1-B213-94ED-01234ABCDEF
```
On Nitro instances, the following command can be used:  

```
[ec2-user ~]$ cat /sys/devices/virtual/dmi/id/board_asset_tag
```
This returns the instance ID, which is unique to EC2 instances:  

```
i-0af01c0123456789a
```