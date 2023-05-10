# aws-certified-developer-associate-certification-notes

### What is the AWS?

AWS (Amazon Web Services) is a Cloud Provider that provides you with servers and services that you can use **on demand** and **scale easily**.

### Getting started with AWS

- AWS Regions: clusters of data centers, most services are region-specific
- AWS Zones: distinct locations within a region
- Data centers: physical buildings housing infrastructure
- Points of presence (PoP): close to end-users, e.g., CloudFront (CDN), Route 53 (DNS), global services

### AWS Global Infrastructure

- AWS Regions: Regions refer to geographically distributed areas where AWS infrastructure is established, consisting of isolated, multiple data centers called Availability Zones to provide low-latency access and comply with regional regulations.

- AWS Availability Zones: Availability Zones are independent data centers within a region, each possessing separate power, cooling, and networking to ensure high availability and fault tolerance, enabling customers to achieve redundancy by distributing applications and data across them.

- AWS Data Centers: Data centers are secure, reliable, and high-performing physical facilities housing AWS infrastructure such as servers and storage devices, designed with redundant power, cooling, and network connections to maintain high uptime.

- AWS Edge Locations / Points of Presence: Edge Locations, or Points of Presence, are smaller infrastructure components deployed close to end-users to provide low-latency access to AWS services like Amazon CloudFront, Lambda@Edge, and Route 53, caching and serving content from the nearest location for improved user experience.

### Selecting an AWS service

- Compliance & data governance: ensures data stays within region unless explicitly permitted
- Customer proximity: reduces latency by being closer to users
- Regional service availability: note that not all services/features are in every region
- Pricing transparency: consider regional cost differences, check service pricing pages


### Identity and Access Management (IAM)

- IAM stands for identity and access management and is a global service in AWS.
- Users represent individuals in an organization and can be grouped together.
- Groups can only contain users, not other groups.
- Users or groups can be assigned IAM policies, which define permissions for accessing AWS services.
- The least privilege principle should be applied to ensure users only have the permissions they need.
- Creating users and groups and assigning policies is important for controlling access to AWS services.

#### IAM Policies

- IAM policies: control access for users/groups/roles
- Group-level policy: applies to all members (e.g., Alice, Bob, Charles)
- Different groups: different policies (e.g., developers, operations)
- Inline policy: attached to individual users (e.g., Fred)
- Users can belong to multiple groups, inherit multiple policies (e.g., Charles, David)


> IAM policy structure:
>  - Version: policy language version (e.g., 2012-10-17)
>  - ID: optional identifier for the policy
>  - Statements: define permissions (can have multiple)
>    - Sid: optional statement ID
>    - Effect: "Allow" or "Deny"
>    - Principal: accounts, users, or roles the policy applies to
>    - Action: list of allowed/denied API calls
>    - Resource: list of resources actions apply to
>    - Condition: optional, when statement should be applied


```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::account-id:root"
        ]
      },
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::mybucket/*"
      ]
    }
  ]
}
```


- Key exam concepts:
  - Effect: Determines whether the policy statement allows or denies access. Specify "Allow" to grant permissions or "Deny" to restrict them.
  - Principal: Defines the AWS accounts, users, or roles to which the policy applies. Understand how to target specific entities and use wildcards or conditions to control access more granularly.
  - Action: Lists the specific AWS API calls that are allowed or denied by the policy statement. Familiarize yourself with common actions and how to use wildcards to grant broader or more specific permissions.
  - Resource: Specifies the AWS resources (e.g., S3 bucket, EC2 instance) to which the actions apply. Know how to identify resources using ARNs (Amazon Resource Names) and use wildcards or conditions for more control.
  - Condition (optional): Defines additional criteria that must be met for the policy statement to be applied. Understand how to use conditions to enforce security best practices, such as MFA requirements, IP restrictions, or time-based access limitations.
