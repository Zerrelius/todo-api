#### Cloudformation Script for basic VPC with public Subnet and a S3 Bucket with Policy to acces from Ec2 and a EC2 Instance with running Python ImageViewer Application
---
Requirements:
* Basic VPC with:
  * Name: ImageViewerVPC
  * CIDR: 10.0.0.0/24
* Subnet:
    * Name: ImageViewerSubnet
    * CIDR: 10.0.0.10/28
    * VPC: Reference to VPCID
* Internet Gateway:
    * Name: ImageViewerIGW
* Internet Gateway Attachment
    * VPC: Reference to VPC
* Route Table:
    * Name: ImageViewerRT
    * VPC: Reference to VPC
* Routes:
    * Name: ImageViewerRoute
    * RouteTable: Reference to RouteTable
    * Dest. Cidr: 0.0.0.0/0
    * IGW: Reference to IGWID
* SecurityGroup:
    * Name: ImageViewerSG
    * Description: SG für Inbound and Outbound Traffic
    * VPC: Reference to VPCID
    * Ingress:
        * Port: 22
        * From: Anywhere
        * Protocoll: tcp
        * Port: 8000
        * From: Anywhere
        * Protocoll: tcp
    * Egress:
        * Port: All
        * From: Anywhere
* S3-Bucket
    * Name: {STACKNAME}.12.11.2024
* I-AM-Role: 
    * Name: EC2-Instance-Role
    * assume-role-policy-document
        * version
        * statement
            * effect: allow
            * service: ec2-service
            * action: sts assume-role
    * Policy: 
        * S3-Read-Access
        * effect: allow
        * action: get object, list-bucket
        * referenz: for S3-bucket
* Instance-Profile:
    * Properties:
        * Roles: ! Ref EC2-Instance-Role
* EC2 Instance:
    * Name: ImageViewerEC2
    * AMI: Ubuntu 24.04
    * InstanceType: T2.micro
    * SecurityGroup: Reference to SecurityGroup
    * IAM-Instance-Profile: ! Ref Instance-Profile
    * Subnet: Reference to SUBNETID
    * UserDataScript: 
        * update & upgrade
        * globale variable für s3-bucket
        * fast api 
        * wget zum "release" auf github
