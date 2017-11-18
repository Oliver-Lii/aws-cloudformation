# aws-cloudformation
Cloudformation templates to help with deploying resources in AWS

## VPC

VPCStack.yml - This template deploys a VPC with public and private subnets. A security group for management access and a DHCP Option Set for the VPC is also created. Further details for the template can be found in the readme.md in the VPC folder

## EC2

AMILookupStack.yml - This template deploys resources required to use the [Amazon AMI lookup function](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/walkthrough-custom-resources-lambda-lookup-amiids.html "Amazon AMI lookup function")

# Authors
- Oliver Li