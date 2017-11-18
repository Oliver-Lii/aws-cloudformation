# VPC
Cloudformation templates to help with deploying EC2 resources in AWS

## VPCStack.yml
This template deploys a VPC with public and private subnets with Internet and NAT gateways. Additionally a security group for management access is created along with a DHCP Option Set for the VPC.

### VPC parameters

The parameters for the VPC. To use a third AZ both public and private CIDR IP address blocks must be supplied. 

| Parameter          | Default | Restrictions              | Required? | Description                                                                                                                                                                                                                |
|--------------------|---------|---------------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AvailabilityZone1  | None    | AWS EC2 Availability Zone | Yes       | First availability zone to be used by the deployed VPC                                                                                                                                                                     |
| AvailabilityZone2  | None    | AWS EC2 AvailabilityZone  | Yes       | Second availability zone to be used by the deployed VPC                                                                                                                                                                    |
| AvailabilityZone3  | None    | AWS EC2 AvailabilityZone  | Yes       | Third availability zone to be used by the deployed VPC. Due to restrictions with Cloudformation templates a value must be selected but will not be used unless values for Private and Public C CIDR IP blocks are provided |
| CidrBlock          | None    | IP address in CIDR format | Yes       | The CIDR IP address block for the whole VPC. This must encompass the private and public subnets to be deployed within the VPC.                                                                                             |
| PrivateSubnetACIDR | None    | IP address in CIDR format | Yes       | CIDR IP address block to be use for first private subnet                                                                                                                                                                   |
| PrivateSubnetBCIDR | None    | IP address in CIDR format | Yes       | CIDR IP address block to use for the second private subnet                                                                                                                                                                 |
| PrivateSubnetCCIDR | None    | IP address in CIDR format | No        | CIDR IP address block to use for the third private subnet. Must be supplied with the Public subnet C CIDR parameter for a third availability zone to be used                                                               |
| PublicSubnetACIDR  | None    | IP address in CIDR format | Yes       | CIDR IP address block to use for the first public subnet                                                                                                                                                                   |
| PublicSubnetBCIDR  | None    | IP address in CIDR format | Yes       | CIDR IP address block to use for the second public subnet                                                                                                                                                                  |
| PublicSubnetCCIDR  | None    | IP address in CIDR format | No        | CIDR IP address block to use for the third private subnet. Must be supplied with the Private subnet C CIDR parameter for a third availability zone to be used                                                              |

### Management Parameters

Parameters for a management security group created with the VPC. The management security group is intended to be attached to EC2 instances to allow access to them from a common IP address range.

| Parameter            | Default | Restrictions              | Required? | Description                                                                       |
|----------------------|---------|---------------------------|-----------|-----------------------------------------------------------------------------------|
| ManagementCIDRIP     | None    | IP address in CIDR format | Yes       | A IP address block which will be used to manage the EC2 instances inside this VPC |
| ManagementAccessType | RDP     | RDP or SSH                | No        | Whether the type of access required is RDP or SSH                                 |

### DHCP Option Parameters

Parameters for the DHCP option set which will be attached to the VPC.

| Parameter         | Default | Restrictions                         | Required? | Description                                                                                                                                                                                           |
|-------------------|---------|--------------------------------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DHCPOptDomainName | None    | String                               | No        | Domain name which should be added to the DHCP Option set. If none supplied then the value ".compute.internal" prefixed with the AWS Region will be used. E.g. "eu-west-1.compute.internal"            |
| DHCPOptDNSServers | None    | Comma separated list of IP addresses | No        | IP addresses of the DNS Servers. The value "AmazonProvidedDNS" will be appended to the list if IP addresses supplied. If no IP addresses supplied "AmazonProvidedDNS" is added to the list by default |
| DHCPOptNTPServers | None    | Comma separated list of IP addresses | No        | IP addresses of NTP servers.                                                                                                                                                                          |

### Outputs

The following are all the outputs from the CloudFormation template and these values are all exported as with the stack name prefixed with a hyphen. E.g. vpc-stack-AvailabilityZone1

| Output Name               | Always Output? | Description                                                             |
|---------------------------|----------------|-------------------------------------------------------------------------|
| AvailabilityZone1         | Yes            | Name of AvailabilityZone 1                                              |
| AvailabilityZone2         | Yes            | Name of AvailabilityZone 2                                              |
| AvailabilityZone3         | No             | Name of AvailabilityZone 3. Only output if a third AZ is deployed       |
| VPCId                     | Yes            | Logical ID of VPC                                                       |
| PrivateSubnetAId          | Yes            | ID of Private Subnet A                                                  |
| PrivateSubnetBId          | Yes            | ID of Private Subnet B                                                  |
| PrivateSubnetCId          | No             | ID of Private Subnet C. Only output if a third AZ is deployed           |
| PublicSubnetAId           | Yes            | ID of Public Subnet A                                                   |
| PublicSubnetBId           | Yes            | ID of Public Subnet B                                                   |
| PublicSubnetCId           | No             | ID of Public Subnet C. Only output if a third AZ is deployed            |
| PrivateSubnetACIDRIp      | Yes            | CIDR IP Range of Private Subnet A                                       |
| PrivateSubnetBCIDRIp      | Yes            | CIDR IP Range of Private Subnet B                                       |
| PrivateSubnetCCIDRIp      | No             | CIDR IP Range of Private Subnet C. Only output if a third AZ deployed   |
| PublicSubnetACIDRIp       | Yes            | CIDR IP Range of Public Subnet A                                        |
| PublicSubnetBCIDRIp       | Yes            | CIDR IP Range of Public Subnet B                                        |
| PublicSubnetCCIDRIp       | No             | CIDR IP Range of Public Subnet C. Only output if a third AZ deployed    |
| ManagementSecurityGroupId | Yes            | ID of the Management Security Group (used for controlling access)       |
| NatGatewayAEIP            | Yes            | Elastic IP of NAT Gateway A                                             |
| NatGatewayBEIP            | Yes            | Elastic IP of NAT Gateway B                                             |
| NatGatewayCEIP            | No             | Elastic IP of NAT Gateway C. Only output if a third AZ deployed         |
| DHCPOptionSetID           | Yes            | ID of the DHCP Option Group Set                                         |
| DHCPOptionSetDomainName   | No             | The domain name set in DHCP Option Set. Only output if a value supplied |
| DHCPOptionSetDNSServerIPs | No             | IP addresses of the DNS Servers. Only output if a value supplied        |
| DHCPOptionSetNTPServerIPs | No             | IP addresses of the NTP Servers. Only output if a value supplied        |
| ThirdAZDeployed           | Yes            | Outputs True or False depending on if a third AZ has been deployed      |


# Authors
- Oliver Li