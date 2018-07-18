title: AWS - Cloudformation for EC2
date: 2018-07-08 11:13:23
tags:
- AWS
- CloudFormation
- EC2
---

ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com


# Cloudformation for EC2

https://github.com/awslabs/aws-cloudformation-templates/blob/master/aws/services/EC2/EC2InstanceWithSecurityGroupSample.yaml


```yaml
AWSTemplateFormatVersion: '2010-09-09'
Metadata:
  License: Apache-2.0
Description: 'AWS CloudFormation Sample Template EC2InstanceWithSecurityGroupSample:
  Create an Amazon EC2 instance running the Amazon Linux AMI. The AMI is chosen based
  on the region in which the stack is run. This example creates an EC2 security group
  for the instance to give you SSH access. **WARNING** This template creates an Amazon
  EC2 instance. You will be billed for the AWS resources used if you create a stack
  from this template.'
Parameters:
  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instance
    Type: AWS::EC2::KeyPair::KeyName
    ConstraintDescription: must be the name of an existing EC2 KeyPair.
  InstanceType:
    Description: WebServer EC2 instance type
    Type: String
    Default: t2.small
    AllowedValues: [t1.micro]
    ConstraintDescription: must be a valid EC2 instance type.
  SSHLocation:
    Description: The IP address range that can be used to SSH to the EC2 instances
    Type: String
    MinLength: 9
    MaxLength: 18
    Default: 0.0.0.0/0
    AllowedPattern: (\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/(\d{1,2})
    ConstraintDescription: must be a valid IP CIDR range of the form x.x.x.x/x.
Mappings:
  AWSInstanceType2Arch:
    t1.micro:
      Arch: PV64
  AWSInstanceType2NATArch:
    t1.micro:
      Arch: NATPV64
  AWSRegionArch2AMI:
    us-east-1:
      PV64: ami-2a69aa47
      HVM64: ami-6869aa05
      HVMG2: ami-50b4f047
    us-east-2:
      PV64: NOT_SUPPORTED
      HVM64: ami-f6035893
      HVMG2: NOT_SUPPORTED
Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref 'InstanceType'
      SecurityGroups: [!Ref 'InstanceSecurityGroup']
      KeyName: !Ref 'KeyName'
      ImageId: !FindInMap [AWSRegionArch2AMI, !Ref 'AWS::Region', !FindInMap [AWSInstanceType2Arch,
          !Ref 'InstanceType', Arch]]
  InstanceSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 22
        ToPort: 22
        CidrIp: !Ref 'SSHLocation'
Outputs:
  InstanceId:
    Description: InstanceId of the newly created EC2 instance
    Value: !Ref 'EC2Instance'
  AZ:
    Description: Availability Zone of the newly created EC2 instance
    Value: !GetAtt [EC2Instance, AvailabilityZone]
  PublicDNS:
    Description: Public DNSName of the newly created EC2 instance
    Value: !GetAtt [EC2Instance, PublicDnsName]
  PublicIP:
    Description: Public IP address of the newly created EC2 instance
    Value: !GetAtt [EC2Instance, PublicIp]
```
