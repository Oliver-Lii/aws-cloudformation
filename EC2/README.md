# EC2
Cloudformation templates to help with deploying EC2 resources in AWS

## AMILookupStack.yml

AMILookupStack.yml - This template deploys resources required to use the [Amazon AMI lookup function](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/walkthrough-custom-resources-lambda-lookup-amiids.html "Amazon AMI lookup function")

### Prerequisites

1.  S3 bucket
2.  [amilookup.zip](https://s3.amazonaws.com/cloudformation-examples/lambda/amilookup-win.zip "Amazon AMI Lookup Package") package in the above S3 bucket

### Parameters

| Parameter  | Default                  | Restrictions                                                                                                                               | Required? | Description                                             |
|------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-----------|---------------------------------------------------------|
| ModuleName | amilookup-win            | String                                                                                                                                     | Yes       | Name of the javascript file                             |
| S3Bucket   | None                     | [AWS S3 bucket name requirements](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-s3-bucket-naming-requirements.html) | Yes       | Name of the S3 bucket containing the amilookup zip file |
| S3Key      | lambda/amilookup-win.zip | string                                                                                                                                     | Yes       | Name and path of the amilookup zip file in the bucket   |

### IAM Role

Capability Named IAM is required. The IAM Role for the Lambda AMI Lookup function is interpolated from the stack name and the AWS region in which the stack is deployed in.

### Outputs

The following are all the outputs from the CloudFormation template and these values are all exported with the stack name prefixed with a hyphen. E.g. ami-lookup-win-stack-AMILookupInfoFunctionArn

| Output Name                    | Always Output? | Description                               |
|--------------------------------|----------------|-------------------------------------------|
| AMIInfoLambdaExecutionRoleName | Yes            | The name of the Lambda Execution IAM Role |
| AMIInfoLambdaExecutionRoleArn  | Yes            | The Arn of the Lambda Execution IAM Role  |
| AMIInfoFunctionArn             | Yes            | The Arn of the Lambda function            |

### Usage

To use the function after the stack has been deployed you need create a custom resource in a EC2 Cloudformation template. 

```
AMIInfo:
Type: Custom::AMIInfo
Properties:
    ServiceToken: !ImportValue {"Fn::Sub" : "${AMILookupStackName}-AMIInfoFunctionArn" }
    Region: !Ref "AWS::Region"
    OSName: !Ref WindowsVersion
```

The EC2 Cloudformation template should have a stack parameter with the name "AMILookupStackName" using the name of the stack created from the AMILookupStack.yml file. A parameter for WindowsVersion should also be specified. As of 18/11/2017 the Amazon AMI lookup package supports the following Windows versions:

* Windows Server 2008 SP2 English 32-bit
* Windows Server 2008 SP2 English 64-bit
* Windows Server 2008 R2 English 64-bit
* Windows Server 2012 RTM English 64-bit
* Windows Server 2012 R2 English 64-bit

The image id can be retrieved in the template as follows:

```
SampleInstance:
  Type: AWS::EC2::Instance
  Properties:
    InstanceType: !Ref InstanceType
    ImageId: !GetAtt AMIInfo.Id
```

# Authors
- Oliver Li