# Getting Started with AWS

## Table of Contents

- [Getting Started with AWS](#getting-started-with-aws)
  - [Table of Contents](#table-of-contents)
  - [What is the AWS?](#what-is-the-aws)
  - [AWS Global Infrastructure](#aws-global-infrastructure)
  - [Selecting an AWS service](#selecting-an-aws-service)

## What is the AWS?

AWS (Amazon Web Services) is a Cloud Provider that provides you with servers and services that you can use **on demand** and **scale easily**.

## AWS Global Infrastructure

1. AWS Regions:

   - clusters of data centers, most services are region-specific
   - Regions refer to geographically distributed areas where AWS infrastructure is established, consisting of isolated, multiple data centers called Availability Zones to provide low-latency access and comply with regional regulations.

2. AWS Availability Zones:

   - distinct locations within a region
   - Availability Zones are independent data centers within a region, each possessing separate power, cooling, and networking to ensure high availability and fault tolerance, enabling customers to achieve redundancy by distributing applications and data across them.

3. AWS Data Centers:

   - physical buildings housing infrastructure
   - Data centers are secure, reliable, and high-performing physical facilities housing AWS infrastructure such as servers and storage devices, designed with redundant power, cooling, and network connections to maintain high uptime.

4. AWS Edge Locations / Points of Presence (PoP):
   - close to end-users, e.g., CloudFront (CDN), Route 53 (DNS), global services
   - Edge Locations, or Points of Presence, are smaller infrastructure components deployed close to end-users to provide low-latency access to AWS services like Amazon CloudFront, Lambda@Edge, and Route 53, caching and serving content from the nearest location for improved user experience.

## Selecting an AWS service

- Compliance & data governance: ensures data stays within region unless explicitly permitted
- Customer proximity: reduces latency by being closer to users
- Regional service availability: note that not all services/features are in every region
- Pricing transparency: consider regional cost differences, check service pricing pages

[Previous: Introduction](./introduction.md) | [Back to main page](../README.md) | [Next: IAM (Identity and Access Management)](./iam-identity-and-access-management.md)
