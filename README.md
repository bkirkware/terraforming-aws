# How Does One Use This?

## What Does This Do?

A booted ops-manager plus a whole BOATLOAD of other goodies, including:

- Friendly DNS entries brought to you by Route53
- An RDS
- A VPC, with security groups
- A whole mess of s3 buckets (5 in total - for all your storage needs)
- An amazing NAT Box
- SSH/HTTPS/TCP ELBs
- An IAM User

## Looking to setup a different IAAS

We have have other terraform templates to help you!

- [azure](https://github.com/pivotal-cf/terraforming-azure)
- [gcp](https://github.com/pivotal-cf/terraforming-gcp)

This list will be updated when more infrastructures come along.

## Prerequisites

### Install Terraform
```bash
go get -u github.com/hashicorp/terraform
```

### AWS Permissions
- AmazonEC2FullAccess
- AmazonRDSFullAccess
- AmazonRoute53FullAccess
- AmazonS3FullAccess
- AmazonVPCFullAccess
- IAMFullAccess

## Notes

You can choose whether you would like an RDS or not. By default we have
`rds_instance_count` set to `0` but setting it to 1 will deploy an RDS.

RDS instances take FOREVER to deploy, keep that in mind.

## Variables

- env_name: **(required)** An arbitrary unique name for namespacing resources
- access_key **(required)** Your Amazon access_key, used for deployment
- secret_key: **(required)** Your Amazon secret_key, also used for deployment
- region: **(required)** Region you want to deploy your resources to
- availability_zones: **(required)** List of AZs you want to deploy to
- dns_suffix: **(required)** Domain to add environment subdomain to

## Ops Manager (optional)
- ops_manager: **(default: true)** Set to false if you don't want an Ops Manager
- ops_manager_ami: **(optional)**  Ops Manager AMI, get the right AMI according to your region from the AWS guide downloaded from [Pivotal Network](https://network.pivotal.io/products/ops-manager)
- optional_ops_manager: **(default: false)** Set to true if you want an additional Ops Manager (useful for testing upgrades)
- optional_ops_manager_ami: **(optional)**  Additional Ops Manager AMI, get the right AMI according to your region from the AWS guide downloaded from [Pivotal Network](https://network.pivotal.io/products/ops-manager)
- ops_manager_instance_type: **(default: m4.large)** Ops Manager instance type

## RDS (optional)
- rds_instance_count: **(default: 0)** Whether or not you would like an RDS for your deployment
- rds_instance_class: **(default: db.m4.large)** Size of the RDS to deploy
- rds_db_username: **(default: admin)** Username for RDS authentication

## Isolation Segments (optional)
- create_isoseg_resources *(optional)* Set to 1 to create HTTP load-balancer across 3 zones for isolation segments. If set, the following 2 isoseg variables are also required.
- isoseg_ssl_cert: *(optional)* SSL certificate for HTTP load balancer configuration. Can be either trusted or self-signed.
- isoseg_ssl_cert_private_key:  *(optional)* Private key for above SSL certificate.

## Running

### Standing up environment

```bash
terraform apply \
  -var "env_name=durian" \
  -var "access_key=access-key-id" \
  -var "secret_key=secret-access-key" \
  -var "region=us-west-1" \
  -var "availability_zones=[\"us-west-1a\", \"us-west-1b\"]" \
  -var "ops_manager_ami=ami-2e02454e" \
  -var "rds_instance_count=1" \
  -var "dns_suffix=example.com"
```

### Tearing down environment

```bash
terraform destroy \
  -var "env_name=durian" \
  -var "access_key=access-key-id" \
  -var "secret_key=secret-access-key" \
  -var "region=us-west-1" \
  -var "availability_zones=[\"us-west-1a\", \"us-west-1b\"]" \
  -var "ops_manager_ami=ami-2e02454e" \
  -var "rds_instance_count=1" \
  -var "dns_suffix=example.com"
```
