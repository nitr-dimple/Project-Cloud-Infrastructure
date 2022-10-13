# infrastructure

Name: Dimpleben Kanjibhai Patel <br>
NUID: 002965372
<br>

This repository contains the infrastructure for cloudformation which does following infrastructure setup:
- Creates Virtual Private Cloud (VPC)
- Creates 3 subnets with different availability zones
- Creates Internet Gateway and attach this Internet Gateway to the Virtual Private Cloud
- Creates public route table and attach all subnets to the route table
- Creates public routes in the public route table with destination CIDR block as 0.0.0.0/0 and internet gateway as target.


## Initial Setup Steps:
- Create AWS account which will be root account using your gmail account.
- Create two alias accounts that will be dev and demo with the same email address.
- Send inivitation to join the organization from root account to dev and demo account.

## Create IAM users and groups
- Login to the root account
- Search IAM service in the search bar
- Create group with csye6225-ta name and with ReadOnlyAccess permission
- Create IAM users for TAs using console access and add them to this group
- Create your own IAM account with AdministratorAccess.
- Assign MFA

Create the IAM users and groups in the dev and demo accounts by following the same steps as that for the root account.

## Steps to setup profiles
- Download [AWS cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
- Create dev profiles using below command
     ```
     aws configure --profile dev
     ```
- Enter the Access Key ID and Secret Key that is generated while creating user IAM account with AdministratorAccess
- Enter region name as us-east-1 and output format as json
- Do same for creating demo profile


## Steps to create CloudFormation:
- Clone the respository by using the following command
   ```
   git clone git@github.com:dimple-northeastern/infrastructure.git
   ```
- Open aws cli and navigate to the folder you cloned.
- For setting up network infrastructure by using default values, run the following command
   ```
   aws cloudformation deploy --profile dev --template-file csye6225-infra.yml --stack-name mystack
   ```
- For setting up network infrastructure by passing the values in parameters, run the following command
    ```
    aws cloudformation --profile dev create-stack --stack-name mystack1 --template-body file://csye6225-infra.yml --parameter ParameterKey=VpcCIDR,ParameterValue="20.0.0.0/16" ParameterKey=PublicSubnet1CIDR,ParameterValue="20.0.1.0/24" ParameterKey=PublicSubnet2CIDR,ParameterValue="20.0.2.0/24" ParameterKey=PublicSubnet3CIDR,ParameterValue="20.0.3.0/24"
   ```

   ```
    aws cloudformation --profile dev create-stack --stack-name mystack2 --template-body file://csye6225-infra.yml --parameter ParameterKey=VpcCIDR,ParameterValue="30.0.0.0/16" ParameterKey=PublicSubnet1CIDR,ParameterValue="30.0.1.0/24" ParameterKey=PublicSubnet2CIDR,ParameterValue="30.0.2.0/24" ParameterKey=PublicSubnet3CIDR,ParameterValue="30.0.3.0/24"
    ```
- In order to delete all the resources associated with the stack, run the following command
    ```
    aws cloudformation --profile dev delete-stack --stack-name mystack
    ```
- For creating VPC network in different region, run the following command
  ```
    aws cloudformation --profile dev create-stack --stack-name mystack1 --template-body file://csye6225-infra.yml --parameter ParameterKey=VpcCIDR,ParameterValue="20.0.0.0/16" ParameterKey=PublicSubnet1CIDR,ParameterValue="20.0.1.0/24" ParameterKey=PublicSubnet2CIDR,ParameterValue="20.0.2.0/24" ParameterKey=PublicSubnet3CIDR,ParameterValue="20.0.3.0/24" --region us-east-1
  ```
- In order to delete all the resources associated with the stack in a specified region, run the following command
   ```
   aws cloudformation --profile dev delete-stack --stack-name mystack2 --region us-east-2
   ```