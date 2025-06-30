Here’s a polished version of your AWS CodeDeploy setup process in a GitHub-friendly **Markdown documentation** format. This can be used as a `README.md` or wiki documentation for your repository.

---

# 🚀 AWS CodeDeploy Setup Guide

This guide walks through deploying an EC2 instance with AWS CloudFormation and CodeDeploy.

---

## 🏗️ Step 1: Create a CodeDeploy Application

1. Navigate to the [AWS CodeDeploy Console](https://console.aws.amazon.com/codedeploy/).
2. Click on **Create application**.
3. Choose **Compute Platform** as **EC2/On-premises**.
4. Provide an Application Name and click **Create application**.


<img width="598" alt="Screenshot 2025-06-30 at 11 46 50 PM" src="https://github.com/user-attachments/assets/d4a14cfb-e6a8-4840-af2e-e9d93e850078" />

---

## 📦 Step 2: Launch Infrastructure with CloudFormation

### 🔧 Upload CloudFormation Template

Use the following YAML template to create a CloudFormation stack. You can save it as `infrastructure.yml` and upload it in the AWS Console when creating a stack.

```yaml
---

AWSTemplateFormatVersion: 2010-09-09

Parameters:

  AmazonLinuxAMIID:

    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2

  MyIP:

    Type: String
    Description: My IP address e.g. 1.2.3.4/32 for Security Group HTTP access rule. Get your IP from http://checkip.amazonaws.com/.
    AllowedPattern: (\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/32
    ConstraintDescription: must be a valid IP address of the form x.x.x.x/32

Resources:

  VPC:

    Type: AWS::EC2::VPC

    Properties:

      CidrBlock: 10.11.0.0/16
      EnableDnsHostnames: **true**
      EnableDnsSupport: **true**

      Tags:
        - Key: 'Name'
          Value: !Join ['', [!Ref 'AWS::StackName', '::VPC'] ]

  InternetGateway:

    Type: AWS::EC2::InternetGateway

    Properties:

      Tags:
        - Key: 'Name'
          Value: !Join ['', [!Ref 'AWS::StackName', '::InternetGateway'] ]

  VPCGatewayAttachment:

    Type: AWS::EC2::VPCGatewayAttachment

    Properties:

      VpcId: !Ref VPC
      InternetGatewayId: !Ref InternetGateway

  PublicSubnetA:

    Type: AWS::EC2::Subnet

    Properties:

      AvailabilityZone: !Select 

        - 0
        - Fn::GetAZs: !Ref 'AWS::Region'

      VpcId: !Ref VPC
      CidrBlock: 10.11.0.0/20
      MapPublicIpOnLaunch: **true**

      Tags:

        - Key: 'Name'
          Value: !Join ['', [!Ref 'AWS::StackName', '::PublicSubnetA'] ]

  PublicRouteTable:

    Type: AWS::EC2::RouteTable

    Properties:

      VpcId: !Ref VPC

      Tags:

        - Key: 'Name'
          Value: !Join ['', [!Ref 'AWS::StackName', '::PublicRouteTable'] ]

  PublicInternetRoute:

    Type: AWS::EC2::Route
    DependsOn: VPCGatewayAttachment

    Properties:

      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway
      RouteTableId: !Ref PublicRouteTable

  PublicSubnetARouteTableAssociation:

    Type: AWS::EC2::SubnetRouteTableAssociation

    Properties:

      RouteTableId: !Ref PublicRouteTable
      SubnetId: !Ref PublicSubnetA

  PublicSecurityGroup:

    Type: AWS::EC2::SecurityGroup

    Properties:

      VpcId:
        Ref: VPC

      GroupDescription: Access to our Web server

      SecurityGroupIngress:

      - Description: Enable HTTP access via port 80 IPv4
        IpProtocol: tcp
        FromPort: '80'
        ToPort: '80'
        CidrIp: !Ref MyIP

      SecurityGroupEgress:

      - Description: Allow all traffic egress
        IpProtocol: -1
        CidrIp: 0.0.0.0/0

      Tags:
        - Key: 'Name'
          Value: !Join ['', [!Ref 'AWS::StackName', '::PublicSecurityGroup'] ]

  ServerRole: 

    Type: AWS::IAM::Role

    Properties: 

      AssumeRolePolicyDocument: 
        Version: "2012-10-17"

        Statement: 
          - 
            Effect: "Allow"

            Principal: 

              Service: 

                - "ec2.amazonaws.com"

            Action: 

              - "sts:AssumeRole"

      Path: "/"

      ManagedPolicyArns:

        - "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"
        - "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"

  DeployRoleProfile: 
    Type: AWS::IAM::InstanceProfile

    Properties: 
      Path: "/"

      Roles: 
        - 
          Ref: ServerRole

  WebServer:
    Type: AWS::EC2::Instance

    Properties:

      ImageId: !Ref AmazonLinuxAMIID
      InstanceType: t2.micro
      IamInstanceProfile: !Ref DeployRoleProfile

      NetworkInterfaces: 

        - AssociatePublicIpAddress: **true**

          DeviceIndex: 0

          GroupSet: 
            - Ref: PublicSecurityGroup

          SubnetId: 
            Ref: PublicSubnetA

      Tags:
        - Key: 'Name'
          Value: !Join ['', [!Ref 'AWS::StackName', '::WebServer'] ]
        - Key: 'role'
          Value: 'webserver'
  
Outputs:
  URL:
    Value:
      Fn::Join:
      - ''
      - - http://
        - Fn::GetAtt:
          - WebServer
          - PublicIp
    Description: NextWork web server

```

### 🌐 Get Your IP Address

To allow HTTP access only from your current IP:

1. Visit: [https://whatismyipaddress.com](https://whatismyipaddress.com)
2. Copy your IP and append `/32`. Example: `123.123.123.123/32`

### ⚠️ Stack Failure Options

When creating the stack, if you encounter a failure:

* Choose **"Delete all newly created resources"** under stack failure options.
* This ensures that any partially created resources are cleaned up automatically.

### Stack Creation Successful

<img width="603" alt="Screenshot 2025-06-30 at 11 47 28 PM" src="https://github.com/user-attachments/assets/1231ebb5-01fe-44d5-aa23-b3c38bed286e" />

---

## 🛠️ Step 3: Create a Deployment Group

Before creating a Deployment Group, you need a **Service Role**.

### 👤 Create a Service Role for CodeDeploy

1. Go to IAM > Roles > Create Role
2. Choose **CodeDeploy** as the use case.
3. Attach the default AWS managed policy (e.g., `AWSCodeDeployRole`).
4. Name your role (e.g., `CodeDeployServiceRole`) and create it.

---

### 🚀 Create Deployment Group

1. In your CodeDeploy application, go to **Deployment Groups > Create Deployment Group**.
2. Choose **EC2 Instances** as the environment configuration.
3. Attach your newly created **Service Role**.
4. **Do not** enable Load Balancing for now.
5. Click **Create Deployment Group**.


---

## 📥 Step 4: Create a Deployment

1. Upload your deployment artifact (.zip) to an S3 bucket.
2. In CodeDeploy, go to your application > **Create deployment**.
3. Paste the **S3 URI** of your artifact in the **Revision Location**.
4. Choose `.zip` as the revision file type.
5. Click **Create Deployment**.

---

## ✅ Deployment Complete!

Your application should now be deployed to your EC2 instance. You can access it using the **Public IP** provided in the CloudFormation Outputs section.

## View the Website

- Copy and paste the Public IP address into the browser.
- *Make sure it is "HTTP" not "HTTPS"

<img width="1185" alt="Screenshot 2025-06-24 at 11 42 26 PM" src="https://github.com/user-attachments/assets/5c26eeb0-2830-4f93-9eb1-6d5c5ad4be13" />

