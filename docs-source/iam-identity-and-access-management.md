# IAM (Identity and Access Management)

## Table of Contents

- [IAM (Identity and Access Management)](#iam-identity-and-access-management)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [IAM Policies](#iam-policies)
    - [IAM Policy Structure](#iam-policy-structure)
    - [IAM Policy Sample](#iam-policy-sample)
  - [IAM Password Policy and MFA](#iam-password-policy-and-mfa)
  - [IAM Roles for Services](#iam-roles-for-services)
  - [ARN](#arn)
  - [IAM Security Tools](#iam-security-tools)

## Overview

- IAM stands for identity and access management and is a global service in AWS.
- Users represent individuals in an organization and can be grouped together.
- Groups can only contain users, not other groups.
- Users or groups can be assigned IAM policies, which define permissions for accessing AWS services.
- The least privilege principle should be applied to ensure users only have the permissions they need.
- Creating users and groups and assigning policies is important for controlling access to AWS services.

## IAM Policies

- IAM policies: control access for users/groups/roles
- Group-level policy: applies to all members (e.g., Alice, Bob, Charles)
- Different groups: different policies (e.g., developers, operations)
- Inline policy: attached to individual users (e.g., Fred)
- Users can belong to multiple groups, inherit multiple policies (e.g., Charles, David)

### IAM Policy Structure

> - `Version:` policy language version (e.g., 2012-10-17)
> - `ID (optional):` identifier for the policy
> - `Statements:` define permissions (can have multiple)
>   - `Sid (optional):` statement ID
>   - `Effect:` "Allow" or "Deny"; Determines whether the policy statement allows or denies access. Specify "Allow" to grant permissions or "Deny" to restrict them.
>   - `Principal:` accounts, users, or roles the policy applies to; Defines the AWS accounts, users, or roles to which the policy applies. Understand how to target specific entities and use wildcards or conditions to control access more granularly.
>   - `Action:` list of allowed/denied API calls; Lists the specific AWS API calls that are allowed or denied by the policy statement. Familiarize yourself with common actions and how to use wildcards to grant broader or more specific permissions.
>   - `Resource:` list of resources actions apply to; Specifies the AWS resources (e.g., S3 bucket, EC2 instance) to which the actions apply. Know how to identify resources using ARNs (Amazon Resource Names) and use wildcards or conditions for more control.
>   - `Condition (optional):` when statement should be applied; Defines additional criteria that must be met for the policy statement to be applied. Understand how to use conditions to enforce security best practices, such as MFA requirements, IP restrictions, or time-based access limitations.

### IAM Policy Sample

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::account-id:root"]
      },
      "Action": "s3:*",
      "Resource": ["arn:aws:s3:::mybucket/*"]
    }
  ]
}
```

## IAM Password Policy and MFA

- Password Policy: Implementing a strong password policy is crucial for account security in AWS. You can set a minimum password length and require specific character types (uppercase letters, lowercase letters, numbers, non-alphanumeric characters). You can allow IAM users to change their own passwords and enforce password expiration to prompt regular password changes. Additionally, password reuse can be prevented.

- MFA (Multi Factor Authentication)
  - MFA adds another layer of security for AWS accounts, especially for Root Accounts and IAM users.
  - MFA combines two factors: a password (something you know) and a security device (something you own).
  - Even if a password is compromised, the account is not, as the attacker would also need the security device.

## IAM Roles for Services

- Introduction
  - IAM Roles are an important component of AWS's Identity and Access Management (IAM) system.
  - IAM Roles are used to assign permissions to AWS services to perform actions on your behalf.
  - Unlike IAM Users, IAM Roles are intended for AWS services, not physical people.
- Usage
  - IAM Roles function similarly to users, in that they need permissions to perform actions.
  - Once an IAM Role is created and assigned to a service (like an EC2 Instance), the service uses the role to access information from AWS.
  - The service can successfully perform the requested action if the IAM Role has the necessary permissions.
- Common Roles
  - EC2 Instance Roles: Give permissions to EC2 Instances (virtual servers) to perform certain actions.
  - Lambda Function Roles: Give permissions to Lambda Functions, which run your code in response to events.
  - Roles for CloudFormation: Give permissions to CloudFormation, a service that helps you model and set up AWS resources.

## ARN

Short for Amazon Resource Name, is a scheme for identifying a specific resource in AWS (Amazon Web Services). It gives the complete name of a resource, which is unique within each AWS account.

The general format of an ARN is as follows:

```bash
arn:partition:service:region:account-id:resource-id
```

- `arn`: Indicates that this is an ARN resource.
- `partition`: Indicates in which AWS partition the resource resides. This is usually "aws" or "aws-cn".
- `service`: Specifies the AWS service to which the resource belongs. For example, it could be "s3" or "iam".
- `region`: Specifies the AWS region where the resource is located. For example, "us-west-2". This field can be left blank in some services.
- `account-id`: Specifies the AWS account to which the resource belongs.
- `resource-id`: Specifies the unique identifier of the resource. This varies from service to service.

Here's an example of an ARN:

```bash
arn:aws:s3:::my_corporate_bucket/exampleobject.png
```

In this example:

- `partition`: "aws"
- `service`: "s3"
- `region`: (Left blank in this case as S3 is a global service)
- `account-id`: (Left blank in this case as S3 buckets can be specific to accounts but can access a bucket owned by a specific account)
- `resource-id`: "my_corporate_bucket/exampleobject.png" (The name of an object in an S3 bucket)

## IAM Security Tools

- IAM Credentials Report (account-level):
  - This is a comprehensive list of all users in your account along with the status of their various credentials.
  - Utilize this tool to monitor and manage user credentials effectively.
- IAM Access Advisor (user-level):
  - This tool displays the service permissions granted to a user and when those services were last accessed.
  - It is especially useful for enforcing the principle of least privilege, as it enables you to identify unused permissions that can be revoked, ensuring users only have necessary access.

[Previous: Getting Started with AWS](./getting-started-with-aws.md) | [Back to main page](../README.md) | [Next: Accessing AWS](./accessing-aws.md)
