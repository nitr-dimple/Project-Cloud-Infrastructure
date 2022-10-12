# infrastructure

#### Name: Dimpleben Kanjibhai Patel
#### NUID: 002965372
<br>

This repository contains the infrastructure for cloudformation which does following infrastructure setup:
- Creates Virtual Private Cloud (VPC)
- Creates 3 subnets with different availability zones
- Creates Internet Gateway and attach this Internet Gateway to the Virtual Private Cloud
- Creates public route table and attach all subnets to the route table
- Creates public routes in the public route table with destination CIDR block as 0.0.0.0/0 and internet gateway as target.

## Steps:
1. Download AWS cli
2. Create dev profiles using below command
     ```
     aws configure --profile dev
     ```
3. Enter the Access Key ID and Secret Key that is generated while creating user IAM account with AdministratorAccess
4. Enter region name as us-east-1 and output format as json
5. Clone the respository by using the following command
   ```
   git clone git@github.com:dimple-northeastern/infrastructure.git
   ```
6. Open aws cli and navigate to the folder you cloned.
7. For setting up network infrastructure by using default values, run the following command
   ```
   aws cloudformation deploy --profile dev --template-file csye6225-infra.yml --stack-name mystack

   ```
8. For setting up network infrastructure by passing the values in parameters, run the following command
    ```
    aws cloudformation --profile dev create-stack --stack-name mystack --template-body file://csye6225-infra.yml --parameter ParameterKey=VpcCIDR,ParameterValue="20.0.0.0/16" ParameterKey=PublicSubnet1CIDR,ParameterValue="20.0.1.0/24" ParameterKey=PublicSubnet2CIDR,ParameterValue="20.0.2.0/24" ParameterKey=PublicSubnet3CIDR,ParameterValue="20.0.3.0/24"
    ```
9. In order to delete all the resources associated with the stack, run the following command
    ```
    aws cloudformation --profile dev delete-stack --stack-name mystack
    ```