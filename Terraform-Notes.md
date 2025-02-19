
#### Git Hub Repo : https://github.com/ashokitschool/Terraform_Projects.git
 
===========
Terraform
===========

=> Developed by Hashicorp

=> To create/provision infrastructure in cloud platform

=> IAC software (infrastructure as code)

=> Supports all most all cloud platforms

=> Terraform will use HCL language to provision infrastructure

		HCL : Hashicorp configuration language

=> We can install terraform in mulitple Operating Systems

	Ex: Windows, Linux....		

==============================
Terraform Vs Cloud Formation
==============================

=> Cloud Formation is used to create infrastructure only in aws cloud

=> Terraform supports all cloud platforms available in the market.


====================================
Terraform Installation in Windows
====================================

Step-1 : Download terraform for windows & extract zip file 

	Note: We can see terraform.exe file

Step-2 : Set path for terraform s/w in System environment variables

Step-3 : Verify terraform setup using cmd

		$ terraform -v

Step-4 : Download and install VS CODE IDE to write terraform scripts

	URL : https://code.visualstudio.com/download


=========================
Terraform Architecture
=========================	

=> Terraform will use HCL script to provision infrastructure in cloud platforms.

=> We need to write HCL script and save it in .tf file


	.tf => init => fmt => validate => plan => apply => destroy


=> Below are the terraform commands

terraform init : Initialize terraform script (.tf file)

terraform fmt : Format terraform script indent spacing (optinal)

terraform validate : Verify terraform script syntax is valid or not (optional)

terraform plan : Create execution plan for terraform script

terraform apply : Create actual resources in cloud based on plan

Note: tfstate file will be created to track the resources created with our script.

terraform destroy : It is used to delete the resources created with our script.


### Terraform AWS Documentation : https://registry.terraform.io/providers/hashicorp/aws/latest/docs


==========================================
Terraform Script To create EC2 Instance
==========================================

provider "aws" {
	
	region = "ap-south-1"
	access_key = "AKIATCKAMNKD6R2YIMPM"
	secret_key = "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"
}

resource "aws_instance" "ashokit_linux_vm" {
	
	ami = "ami-0e53db6fd757e38c7"
	instance_type = "t2.micro"
	key_name = "awslab"
	security_groups = ["default"]
	tags = {
		Name = "LinuxVM"
	}
}
---------------------------------------

$ terraform init

$ terraform validate

$ terraform fmt

$ terraform plan

$ terraform apply --auto-approve

$ terraform destory --auto-approve


=========================
Variables in Terraform
=========================

=> Variables are used to store data in key-value format

			id = 101
			name = ashok

=> We can remove hard coded values from resources script using variables

=> Variables we can maintain in seperate .tf file

	Ex :  input-vars.tf

variable "ami" {
    description = "Amazon machine image id"
    default  = "ami-0e53db6fd757e38c7"
}

variable "instance_type" {
    description = "Represens EC2 instance type"
    default = "t2.micro"
}

variable "key_name" {
    description = ""
    default = "awslab"
}

=> We can access variables in our resources script like below

resource "aws_instance" "ashokit_ec2_vm" {

    ami = "${var.ami}"
    instance_type = "${var.instance_type}"
    key_name = "${var.key_name}"
    security_groups = ["default"]
    tags = {
        Name = "AIT-Linux-VM
    }
}


=================================
Types of variables in terraform
=================================

		1) Input Variables

		2) Output Variables

=> Input variables are used to supply values to the terraform script.

	Ex : ami, instance_type, keyname, securitygrp

=> Output variables are used to get the values from terraform script after execution.

		Ex-1 : After EC2 VM created, print ec2-vm public ip

		Ex-2 : After S3 bucket got created, print bucket info

		Ex-3 : After RDS instance got created, print DB endpoint

		Ex-4 : After IAM user got created print IAM user info

-------------------input-vars.tf------------------

variable "ami" {
    description = "Amazon machine image id"
    default  = "ami-0e53db6fd757e38c7"
}

variable "instance_type" {
    description = "Represens EC2 instance type"
    default = "t2.micro"
}

variable "key_name" {
    description = ""
    default = "awslab"
}

--------------------main.tf-------------------

resource "aws_instance" "ashokit_ec2_vm" {
  ami             = var.ami
  instance_type   = var.instance_type
  key_name        = var.key_name
  security_groups = ["default"]
  tags = {
    Name = "AIT-Linux-VM"
  }
}

---------------output-vars.tf--------------------

output "ec2_vm_public_ip" {
  value = aws_instance.ashokit_ec2_vm.public_ip
}

output "ec2_private_ip" {
  value = aws_instance.ashokit_ec2_vm.private_ip
}

output "ec2_subnet_id"{
    value = aws_instance.ashokit_ec2_vm.subnet_id
}

output "ec2_complete_info"{
    value = aws_instance.ashokit_ec2_vm
}

===================================================
Create Multiple EC2 Instances with Diff Tag names
===================================================

locals {

  instances_count = 3

  instances_tags = [
    {
      Name = "Dev-Server"
    },
    {
      Name = "QA-Server"
    },
    {
      Name = "UAT-Server"
    }
  ]
}

resource "aws_instance" "ashokit_ec2_vm" {
  count           = locals.instances_count
  ami             = var.ami
  instance_type   = var.instance_type
  key_name        = var.key_name
  security_groups = ["default"]
  tags = locals.instances_tags[count.index]
}

output "instance_ids" {	
	value = aws_instance.ashokit_ec2_vm[*].pubic_ip
}

=============
Assignment
=============

1) Create S3 Bucket

2) Create RDS instance

3) Create IAM user

4) Create Custom VPC

5) Create Ec2 Instance using Custom VPC

===================
Terraform Modules
===================

=> A Terraform module is a set of terraform configuration files available in a single directory. 

=> One module can contain one or more .tf files

        01-Project
          - main.tf
          - inputs.tf
          - outputs.tf

=> One module can have any no.of child modules in terraform

    Ex: inside project we can take ec2, s3, rds as child modules

        ashokit
            - ec2
                - main.tf
                - inputs.tf
                - outputs.tf
            - s3
                - main.tf
                - inputs.tf
                - outputs.tf

            - provider.tf
            - main.tf
            - outputs.tf


Note : Using terraform modules we can achieve re-usability

Note: We will run terraform commands from root module and root module will invoke child modules for execution.


======================================
Terraform project setup with Modules
======================================

### Step-1 : Create Project directory

        Ex: 03-App

### Step-2 : Create "modules" directory inside project directory

        Ex: 03-App
                - modules

### Step-3 : Create "ec2" & "s3" directories inside "modules" directory

        Ex : 03-App
                - modules
                    - ec2
                    - s3

### Step-4 : Create terraform scripts inside "ec2" directory

            inputs.tf
            main.tf
            outputs.tf         

### Step-5 : Create terraform scripts inside "s3" directory

            inputs.tf
            main.tf
            outputs.tf          


### Step-6 : create "provider.tf" file in root module


### Step-7 : create "main.tf" file in root module and invoke child modules from root module.

module "my_ec2"{
    source = "./modules/ec2"
}

module "my_s3" {
    source = "./modules/s3"
}


### Step-8: Create "ouputs.tf" in project root module and access child modules related outputs.

output "ait_vm_public_ip"{
    value = module.my_ec2.a1
}

output "ait_vm_private_ip" {
    value = module.my_ec2.a2
}

Note: here a1 and a2 are ouput variables declared in ec2 module ouputs.tf file

================================
Environments of the project
===============================

=> Env means the platform that is required to run our application

    Ex: Servers, Database, Storage, Network....


=> One project contains multiple envs    

        Ex: DEV, SIT or QA, UAT, PILOT, PROD env


Dev Env :  Developers will use it for code integration testing

SIT / QA Env : Testers will use it for System Integration Testing

UAT Env: Client will use it for Acceptance testing.

Pilot Env : Pre-Prod testing and Performance testing.

Prod Env : Live Environment.

===============================================

Non-PROD Env (DEV, SIT, UAT and PILOT)

PROD Env (Very important & critical)

Note: In real-time from environment to environment infrastructure resources configuration might be different

    Ex : For Non-PROD : t2.medium instances required

         For PROD : t2.xlarge instances required

=> In order to achieve this requirement we will maintain environment specific input variable file like below         

===============================================

inputs-dev.tf : input variables for dev env

inputs-sit.tf : input variables for SIT env

inputs-uat.tf : input variables for UAT env

inputs-pilot.tf : input variables for PILOT env

inputs-prod.tf : input variables for PROD env

=================================================

=> When we are executing terraform apply command we can pass inputs variable file like below.

# create infrastructure for DEV env
$ terraform apply --var-file=inputs-dev.tf

# create infrastructure for PROD env
$ terraform apply --var-file=inputs-prod.tf


Note: With this approach we can achieve loosely coupling and we can achieve script re-usability.


=========================
Workspace in terraform
=========================

=> To manage infrastructure for multiple environments we will use Terraform workspace concept.

=> When we use workspace, it will maintain seperate state file for every environment/workspace.

Note: We can execute same script for multiple environments.


$ terraform workspace show

$ terraform workspace new dev

$ terraform workspace new qa

$ terraform workspace new uat

$ terraform workspace new prod

$ terraform worksapce list

$ terraform workspace select dev


==================================
Working with terraform workspaces
==================================

Step-1: Create Terraform Project

Step-2: Create provider.tf file and configure provider details

Step-3: Create input variables files based on environments and configure variable values.

    Ex :

        dev.tfvars
        qa.tfvars
        prod.tfvars

Step-4 : Create main resources script file

Step-6 : Create outputs variable file        

Step-7 : Create Workspaces 

        $ terraform workspace new dev
        $ terraform workspace new qa

Step-8 : Select workspace

        $ terraform workspace select dev

Step-9 : Run script and check state files

        $ terraform apply --var-file=dev.tfvars

Note: When we use workspaces concept, it will maintain seperate state file for every environment.

Step-9 : switch to qa workspace and run the script

        $ terraform workspace select qa

        $ terraform apply --var-file=qa.tfvars

=======================================
What is taint and untaint in terraform
=======================================

=> Terraform "taint" is used to replace the resource when we apply the script next time.

=> For example we have created two resources like below

resource "aws_instance" "vm1"{
    // configuration
}

resource "aws_s3_bucket" "abt1"{
    // configuration
}

=> After sometime we realized that ec2 vm got damaged then we can taint that ec2 vm using below command.

$ terraform taint aws_instance.vm1

$ terraform apply --auto-approve

Note: The alternate for "taint" is "replace"

$ terraform apply -replace="aws_instance.vm1"



=========================
What is Terraform Vault
=========================

=> Provided by Hashicorp org.

=> It is used to manage secrets such as passwords, tokens, any sensitive inform

Ex:
====

1) While creating RDS instance we need to specify username and pwd

2) While creating iAM user we need to specify username and pwd

Note: IT is not recommended to configure those credentials in script directley.

=> Vault allows you to store and manage secrets securely, reducing the risk of exposing sensitive data in your Terraform configurations.


===================
Vault Server Setup
===================

Documentation : https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-install

@@ Step-1 :: Create EC2 Instance (Ubuntu AMI) and connect with that

@@ Step-2 :: Install Vault on the EC2 instance

# Install gpg

sudo apt update && sudo apt install gpg

# Download the signing key to a new keyring

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

# Verify the key's fingerprint

gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint

# add Hashicorp repo to pkg manager

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

# update packages
sudo apt update

# install vault
sudo apt install vault

@@ Step-3 :: Start the vault server (It runs on port number 8200)

$ vault server -dev -dev-listen-address="0.0.0.0:8200"

@@ Step-4 :: Access Vault server UI dashboard (enable 8200 port in inbound rules)

@@ Step-5 : Enable Secret Engine (KV) & create secret and store the data

@@ Step-6 : Connect with Vault server from different ssh terminal

@@ Step-7 : Export Vault addr & Enable approle

	$ export VAULT_ADDR='http://0.0.0.0:8200'
	$ vault auth enable approle

@@ Step-8 : Create Policy

---------------------------------------------
vault policy write terraform - <<EOF
path "*" {
  capabilities = ["list", "read"]
}

path "secrets/data/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

path "kv/data/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}


path "secret/data/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

path "auth/token/create" {
capabilities = ["create", "read", "update", "list"]
}
EOF
------------------------------------------
@@ Step-9 : Create Role
-----------------------------------------
vault write auth/approle/role/terraform \
    secret_id_ttl=10m \
    token_num_uses=10 \
    token_ttl=20m \
    token_max_ttl=30m \
    secret_id_num_uses=40 \
    token_policies=terraform
----------------------------------------

@@ Step-10 : Generate Role_ID and Secret_ID and copy them

$ vault read auth/approle/role/terraform/role-id
$ vault write -f auth/approle/role/terraform/secret-id

@@ Step-11 : Write terraform script and read data from terraform vault server

------------------------------------------------------

provider "aws" {
  region     = "ap-south-1"
  access_key = ""
  secret_key = ""
}

provider "vault" {
  address             = "http://public-ip:8200"
  skip_child_token = true

  auth_login {
    path = "auth/approle/login"
    parameters = {
      role_id   = "<>"
      secret_id = "<>"
    }
  }
}

data "vault_kv_secret_v2" "example" {
  mount = "kv"
  name  = "test-secret"
}

resource "aws_instance" "ashokit_vm" {
  ami           = "ami-08718895af4dfa033"
  instance_type = "t2.micro"
  tags = {
    Secret = data.vault_kv_secret_v2.example.data["tagname"]
  }
}
--------------------------------------------------------------------------------------

Assignment : Create AWS RDS instance by reading db_username and db_pwd from Vault Server.

--------------------------------------------------------------------------------------


========================
Terraform Summary
========================

1) Infrastructure as code (IAC)

2) Terraform Introduction

3) Terraform Setup (Windows)

4) Terraform Architecture

5) Terraform Scripts using HCL

6) Variables (input & output)

7) Terraform modules

8) Project Environments

9) Terraform Workspaces

10) Resource taint or replace

11) Terraform Vault


