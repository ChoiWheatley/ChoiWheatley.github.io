---
aliases: 
tags: 
description:
title: aws cli cheatsheet
created: 2024-11-19T16:18:42
updated: 2024-11-19T16:20:52
---

## EC2 (Elastic Compute Cloud)

### Instances

- List all instances:

  ```bash
  aws ec2 describe-instances
  ```

- Start an instance:

  ```bash
  aws ec2 start-instances --instance-ids <instance-id>
  ```

- Stop an instance:

  ```bash
  aws ec2 stop-instances --instance-ids <instance-id>
  ```

- Reboot an instance:

  ```bash
  aws ec2 reboot-instances --instance-ids <instance-id>
  ```

- Terminate an instance:

  ```bash
  aws ec2 terminate-instances --instance-ids <instance-id>
  ```

### Key Pairs

- List key pairs:

  ```bash
  aws ec2 describe-key-pairs
  ```

- Create a key pair:

  ```bash
  aws ec2 create-key-pair --key-name <key-name>
  ```

### Security Groups

- List security groups:

  ```bash
  aws ec2 describe-security-groups
  ```

- Create a security group:

  ```bash
  aws ec2 create-security-group --group-name <group-name> --description "<description>"
  ```

---

## S3 (Simple Storage Service)

### Buckets

- List all buckets:

  ```bash
  aws s3 ls
  ```

- Create a bucket:

  ```bash
  aws s3 mb s3://<bucket-name>
  ```

- Delete a bucket:

  ```bash
  aws s3 rb s3://<bucket-name> --force
  ```

### Objects

- List objects in a bucket:

  ```bash
  aws s3 ls s3://<bucket-name>/
  ```

- Upload a file:

  ```bash
  aws s3 cp <local-file> s3://<bucket-name>/
  ```

- Download a file:

  ```bash
  aws s3 cp s3://<bucket-name>/<file-name> <local-path>
  ```

- Sync local directory with S3 bucket:

  ```bash
  aws s3 sync <local-dir> s3://<bucket-name>
  ```

- Delete an object:

  ```bash
  aws s3 rm s3://<bucket-name>/<file-name>
  ```

---

## RDS (Relational Database Service)

### Instances

- List RDS instances:

  ```bash
  aws rds describe-db-instances
  ```

- Start an RDS instance:

  ```bash
  aws rds start-db-instance --db-instance-identifier <db-instance-id>
  ```

- Stop an RDS instance:

  ```bash
  aws rds stop-db-instance --db-instance-identifier <db-instance-id>
  ```

- Delete an RDS instance:

  ```bash
  aws rds delete-db-instance --db-instance-identifier <db-instance-id> --skip-final-snapshot
  ```

### Snapshots

- List DB snapshots:

  ```bash
  aws rds describe-db-snapshots
  ```

- Create a DB snapshot:

  ```bash
  aws rds create-db-snapshot --db-instance-identifier <db-instance-id> --db-snapshot-identifier <snapshot-name>
  ```

---

## ECR (Elastic Container Registry)

### Repositories

- List all repositories:

  ```bash
  aws ecr describe-repositories
  ```

- Create a repository:

  ```bash
  aws ecr create-repository --repository-name <repository-name>
  ```

- Delete a repository:

  ```bash
  aws ecr delete-repository --repository-name <repository-name> --force
  ```

### Images

- List images in a repository:

  ```bash
  aws ecr list-images --repository-name <repository-name>
  ```

- Authenticate Docker with ECR:

  ```bash
  aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
  ```

- Push an image to ECR:

  ```bash
  docker push <account-id>.dkr.ecr.<region>.amazonaws.com/<repository-name>:<tag>
  ```

---

## IAM (Identity and Access Management)

### Users

- List users:

  ```bash
  aws iam list-users
  ```

- Create a user:

  ```bash
  aws iam create-user --user-name <user-name>
  ```

- Delete a user:

  ```bash
  aws iam delete-user --user-name <user-name>
  ```

### Roles

- List roles:

  ```bash
  aws iam list-roles
  ```

- Create a role:

  ```bash
  aws iam create-role --role-name <role-name> --assume-role-policy-document file://<policy-file>.json
  ```

- Delete a role:

  ```bash
  aws iam delete-role --role-name <role-name>
  ```

### Policies

- List policies:

  ```bash
  aws iam list-policies
  ```

- Attach a policy to a user:

  ```bash
  aws iam attach-user-policy --user-name <user-name> --policy-arn <policy-arn>
  ```

---

## Credentials

### Configure AWS CLI

- Configure CLI with credentials:

  ```bash
  aws configure
  ```

### Profile Management

- Use a specific profile:

  ```bash
  aws <service> <command> --profile <profile-name>
  ```

- List all profiles:

  ```bash
  cat ~/.aws/credentials
  ```

- Set environment variables for credentials:

  ```bash
  export AWS_ACCESS_KEY_ID=<your-access-key>
  export AWS_SECRET_ACCESS_KEY=<your-secret-key>
  export AWS_DEFAULT_REGION=<your-region>
  ```

### Rotate Access Keys

- List access keys:

  ```bash
  aws iam list-access-keys --user-name <user-name>
  ```

- Create a new access key:

  ```bash
  aws iam create-access-key --user-name <user-name>
  ```

- Delete an access key:

  ```bash
  aws iam delete-access-key --access-key-id <access-key-id> --user-name <user-name>
  ```
