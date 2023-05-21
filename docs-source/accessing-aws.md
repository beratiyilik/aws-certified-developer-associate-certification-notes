# Accessing AWS

## Table of Contents

- [Accessing AWS](#accessing-aws)
  - [Table of Contents](#table-of-contents)
  - [AWS Management Console](#aws-management-console)
    - [AWS CloudShell](#aws-cloudshell)
  - [AWS CLI (Command Line Interface)](#aws-cli-command-line-interface)
    - [Access Keys](#access-keys)
    - [Authentication Methods](#authentication-methods)
    - [Configuration and Credential Files](#configuration-and-credential-files)
      - [Profile Naming](#profile-naming)
      - [Default Profile](#default-profile)
      - [Assuming Roles](#assuming-roles)
  - [AWS SDK (Software Developer Kit)](#aws-sdk-software-developer-kit)
  - [Quiz](#quiz)

## AWS Management Console

A web-based interface, protected by a username, password and possibly multi-factor authentication.

### AWS CloudShell

- CloudShell is an AWS service that provides a browser-based terminal in the cloud to manage AWS resources.
- CloudShell is not available in all AWS regions.
- You can issue AWS CLI commands from within CloudShell, such as listing users using 'aws iam list-users'.
- The default region for API calls in CloudShell is the region you're currently logged in to.
- CloudShell provides a persistent file storage in your home directory.
- Any files you create in your CloudShell environment will persist between sessions.
- Example: create a text file (echo test > demo.txt) and it will stay even if you restart your CloudShell.
- CloudShell allows you to upload and download files.
- To download, get the full path of the file and use the 'Action > Download File' command.
- You can upload files to your CloudShell environment.
- CloudShell allows multiple tabs or split columns for simultaneous sessions.
- The upload and download features of CloudShell can be a lifesaver for many.
- Remember to use a region where CloudShell is available if you want to utilize it.

## AWS CLI (Command Line Interface)

A tool that allows interaction with AWS services from the terminal using commands. It is protected by access keys, which are equivalent to a username and password. The CLI is open-source and provides direct access to the public APIs of AWS services. It can also be used to develop scripts for resource management and task automation.

- Interact with AWS services using commands in the command-line shell.
- Direct access to AWS service APIs.
- Develop scripts for resource management and automation.
- Open-source on GitHub.
- Alternative to the Management Console.

### Access Keys

Access keys in AWS are used to make programmatic calls to AWS from the AWS Command Line Interface (CLI), AWS SDKs, or other AWS services. They consist of two parts: an access key ID and a secret access key.

1. Access Key ID: It's a unique identifier associated with a secret access key. Similar to a username, it is used to identify the party making a request to an AWS service.

2. Secret Access Key: This is the "password" part of the access key. It is important to keep it confidential to protect your AWS account. Unlike the password, it is not used for console logins.

### Authentication Methods

- **Command Line Options:** Overrides other settings. Examples are `--region`, `--output`, and `--profile`.
- **Environment Variables:** Values can be stored in your system's environment variables.
- **Assume Role:** You can assume IAM role permissions through configuration or by using the aws sts assume-role command.
- **Assume Role with Web Identity:** You can assume IAM role permissions using web identity through configuration or aws sts assume-role.
- **AWS IAM Identity Center:** Run aws configure sso to update credentials in the config file.
- **Credentials File:** This file is updated with aws configure. It's located at `~/.aws/credentials` on Linux/macOS or `C:\Users\USERNAME\.aws\credentials` on Windows.
- **Custom Process:** Get credentials from an external source.
- **Configuration File:** This file is updated with aws configure. It's located at `~/.aws/config` on Linux/macOS or `C:\Users\USERNAME\.aws\config` on Windows.
- **Amazon EC2 Instance Profile Credentials:** The IAM role associated with EC2 instances provides temporary credentials.
- **Container Credentials:** The IAM role associated with ECS task definitions provides temporary credentials.

### Configuration and Credential Files

Configuration and credential settings are stored in plaintext files. These files are divided into sections called profiles. A profile is a named collection of settings. Each profile can specify different credentials, AWS regions, and output formats.

`~/.aws/config`

```bash
[default]
region=us-west-2
output=json
```

`~/.aws/credentials`

```bash
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```

#### Profile Naming

- In the config file, the profile name should be prefixed with the word `'profile'`, like `[profile user1]`.
- In the credentials file, do not use the prefix `'profile'`, just write the profile name like `[user1]`.

`~/.aws/config`

```bash
[default]
region=us-west-2
output=json

[profile my-profile]
region=us-east-1
output=text
```

`~/.aws/credentials`

```bash
[default]
aws_access_key_id=YOUR_DEFAULT_ACCESS_KEY
aws_secret_access_key=YOUR_DEFAULT_SECRET_KEY

[my-profile]
aws_access_key_id=YOUR_PROFILE_ACCESS_KEY
aws_secret_access_key=YOUR_PROFILE_SECRET_KEY
```

#### Default Profile

- The first `[default]` profile is used when you run an AWS CLI command without specifying a profile. If you want to use a different profile, you can specify it using the `--profile user1` parameter.

#### Assuming Roles

- You can assume IAM role permissions through configuration or by using the `aws sts assume-role` command.
- Assume Role with Web Identity: You can assume IAM role permissions using web identity through configuration or `aws sts assume-role`.

`~/.aws/config`

```bash
[profile crossaccount]
region = us-west-2
output = json
role_arn = arn:aws:iam::123456789012:role/role_name
source_profile = default
```

`~/.aws/credentials`

```bash
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```

## AWS SDK (Software Developer Kit)

A set of language-specific libraries used to access and manage AWS services programmatically within the application code. Also protected by access keys. It supports a variety of programming languages (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++), mobile SDKs (Android, iOS), and IoT device SDKs (Embedded C, Arduino).

- Language-specific APIs for accessing and managing AWS services programmatically.
- Embedded within applications.
- Supports various programming languages, mobile SDKs (Android, iOS), and IoT device SDKs.
- Example: AWS CLI is built on AWS SDK for Python (Boto).

## Quiz

- **Principle of Least Privilege**: This principle advocates for granting the minimal level of access or permissions necessary for a user to perform their tasks, thus minimizing potential damage in case of a breach.

- **IAM Roles**: IAM roles are AWS entities that have permissions policies determining what the identity can and cannot do in AWS. They are not associated with specific users or groups but can be assumed by trusted entities.

- **IAM Users and User Groups**: IAM users represent a person or service, and User Groups are collections of IAM users. Users don't have to belong to a group, and groups can't contain other groups.

- **IAM Policies**: These are JSON documents that are attached to users, groups, and roles to define their permissions. They determine what actions are allowed or denied on what AWS resources.

- **IAM Security Tools**: AWS provides security tools like the IAM Credentials Report to aid in managing and maintaining security.

- **Best Practices**: Some recommended best practices for AWS IAM include not using the root account for everyday tasks, enabling multi-factor authentication (MFA), and regularly rotating credentials.

- **Shared Responsibility Model**: In AWS's model, AWS is responsible for the security of the cloud (i.e., infrastructure), and customers are responsible for security in the cloud (i.e., managing their resources, including IAM configurations).

- **IAM Statements**: An IAM policy consists of one or more statements that include components like Effect, Principal, Action, Resource, and Condition. The Version is not a part of the statement but of the policy itself.

- **Root Account Security**: For root account security, it's recommended to enable MFA and limit its usage. The root account has full access to all resources in the AWS account and cannot be restricted; therefore, it's crucial to secure it effectively.

[Previous: IAM (Identity and Access Management](./iam-identity-and-access-management.md) | [Back to main page](../README.md) | [Next: EC2 (Elastic Compute Cloud)](./ec2-fundamentals.md)
