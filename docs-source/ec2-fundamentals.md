# EC2 Fundamentals

## Table of Contents

- [EC2 Fundamentals](#ec2-fundamentals)
  - [Table of Contents](#table-of-contents)
  - [AWS Budget Setup](#aws-budget-setup)
  - [EC2 Basics](#ec2-basics)
    - [EC2 Instance Types Basics](#ec2-instance-types-basics)
    - [EC2 Instance Purchasing Options](#ec2-instance-purchasing-options)
  - [Security in EC2](#security-in-ec2)
    - [Security Groups and Classic Ports Overview](#security-groups-and-classic-ports-overview)
    - [EC2 Instance Roles Demo](#ec2-instance-roles-demo)
  - [Access and Connectivity](#access-and-connectivity)
    - [How to SSH using Linux or Mac](#how-to-ssh-using-linux-or-mac)
    - [EC2 Instance Connect](#ec2-instance-connect)
  - [Quiz: EC2 Fundamentals Quiz](#quiz-ec2-fundamentals-quiz)

## AWS Budget Setup

- **Login to Account:** You need to login to your account and go to My Account > My Billing Dashboard.

- **Setting Permissions for IAM Users:** If you are logged in as an IAM user and need permissions, you will have to switch to your root account. Here, scroll down to find the setting 'IAM User and Role Access to Billing Information'. Edit and activate IAM access to allow IAM administrators to access billing data.

- **Understanding the Billing Dashboard:** The AWS Billing Dashboard shows your monthly charges. Go to Bills on the left side, then click on 'try the new bills page experience' to see your charges by service. For example, charges for Elastic Compute Cloud service can be found here.

- **Identifying Source of Charges:** If you notice any charges, you can identify the source by checking the 'Usage Description'. For instance, if the charge is from an unused EBS volume, you can go to the Elastic Compute Cloud console, switch to the respective region, go to the EBS section, and delete the unused volume.

- **Checking Free Tier Usage:** On the left hand side, you can check your free tier usage and current usage for all your services. This helps you keep track of whether you're about to hit the limit or if you're at 100% of your free tier.

- **Setting up AWS Budget:** Go to the budget console and create a budget to track costs and receive alerts if you're about to exceed the limit. You can create a 'zero spend budget' that notifies you once your spending exceeds the free tier limits. You can also set up a 'monthly cost budget' for a set amount (e.g., $10), and you will be notified when your actual spend reaches 85% or 100%, or if your forecasted spend is expected to reach 100%.

- **Conclusion:** With this setup, you can control your costs by exploring your bills and free tier, and setting up a budget to track your monthly cost or free tier cost.

## EC2 Basics

- Overview

  - EC2 is an Infrastructure as a Service (IaaS) offering from Amazon Web Services (AWS).

  - It includes various services like renting virtual machines (EC2 instances), storing data on virtual drives (EBS volumes), load distribution (Elastic Load Balancer), and scaling services (Auto Scaling Group or ASG).

- EC2 Sizing & Configuration Options

  - **Operating System:** Linux, Windows, or Mac OS.
  - **Compute Power:** How much CPU and cores needed.
  - **Memory:** How much RAM needed.
  - **Storage Space:** Network-attached (EBS & EFS) or hardware attached (EC2 Instance Store).
  - **Network Card:** Speed, Public IP address.
  - **Firewall Rules:** Security group.
  - **Bootstrap Script:** EC2 User Data for initial configuration at launch.

- EC2 Instance Types: Different types of EC2 instances are optimized for different use cases.
  - **General Purpose:** Great for diverse workloads such as web servers or code repositories.
  - **Compute Optimized:** Suitable for compute-intensive tasks that require high-performance processors.
  - **Memory Optimized:** Fast performance for workloads that process large data sets in memory.
  - **Storage Optimized:** Great for storage-intensive tasks that require high, sequential read and write access to large data - \*\*sets on local storage.

### EC2 Instance Types Basics

- **Naming Convention:** AWS has a specific naming convention for their EC2 instances, such as m5.2xlarge. Here 'm' is the instance class, '5' is the generation, and '2xlarge' represents the size within the instance class.

- **General Purpose:** These instances are suitable for a range of workloads such as web servers or code repositories. They provide a balance between compute, memory, and networking. Example: t2.micro.

- **Compute Optimized:** These instances are designed for compute-intensive tasks that need high-performance processors. Use cases include batch processing workloads, media transcoding, high-performance computing (HPC), scientific modeling, machine learning, and gaming servers. Example: C series instances.

- **Memory Optimized:** These instances offer fast performance for workloads that process large data sets in memory. Use cases include high-performance relational/non-relational databases, web scale cache stores, in-memory databases optimized for BI (business intelligence), and applications performing real-time processing of big unstructured data. Example: R series instances.

- **Storage Optimized:** These instances are ideal for storage-intensive tasks requiring high, sequential read and write access to large data sets on local storage. Use cases include high frequency OLTP systems, relational & NoSQL databases, in-memory database cache (e.g., Redis), data warehousing applications, and distributed file systems. Example: I, G, and H series instances.

- **Instance Comparison:**

  - t2.micro: 1 vCPU, 1 GiB memory, EBS-Only storage, Low to Moderate network performance.
  - c5d.4xlarge: 16 vCPU, 32 GiB memory, 1 x 400 NVMe SSD storage, Up to 10 Gbps network performance, 4,750 Mbps EBS bandwidth.
  - r5.16xlarge: 64 vCPU, 512 GiB memory, EBS Only storage, 20 Gbps network performance, 13,600 Mbps EBS bandwidth.

- **Reference:** You can use [instances.vantage.sh](https://instances.vantage.sh/) to compare all the EC2 instances available in AWS.

### EC2 Instance Purchasing Options

- Overview

  - **On-Demand Instances:** These are ideal for short-term workloads with predictable pricing. You pay for what you use on a per-second basis. Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave.

  - **Reserved Instances:** These offer a significant discount (up to 72%) compared to On-Demand pricing and are ideal for long-term workloads. They are available in one- or three-year terms, and you reserve a specific instance type, region, tenancy, and operating system. Convertible Reserved Instances offer the flexibility to change these attributes. Recommended for long-term and predictable workloads.

  - **Savings Plans:** These are similar to Reserved Instances but with a commitment to a certain dollar amount of usage per hour, rather than a specific instance type. This offers flexibility across instance size, OS, and tenancy.

  - **Spot Instances:** These offer the highest discounts (up to 90%) but can be terminated by AWS at any time. They're suited for workloads that are resilient to failure, like batch jobs, data analysis, or workloads with a flexible start and end time. Useful for workloads with flexible start and end times, or for workloads that are resilient to failure.

  - **Dedicated Hosts:** This allows you to reserve an entire physical server and control instance placements. It's best suited for cases where you have compliance requirements or server-bound software licenses.

  - **Dedicated Instances:** These run on hardware that’s dedicated to you. Unlike a dedicated host, you might share the hardware with other instances in the same account and have no control over instance placement.

  - **Capacity Reservations:** This allows you to reserve capacity for On-Demand instances in a specific AZ for any duration. You pay the On-Demand rate whether you run instances or not. This is best suited for short-term, uninterrupted workloads in a specific AZ.

- Analogy

  - **On-Demand Instances:** Think of this like a taxi service. You only pay for the taxi when you need it and for the exact duration of the ride. Once your journey is over, you stop paying. This is great for short trips where you need flexibility and don't want to commit to any long-term contracts.

  - **Reserved Instances:** This is like leasing a car for a fixed period of time (one or three years). Even if you don't use the car every day, you still have to pay for it because you've reserved it for your use. This is economical for people who know they will need a car every day for a long period.

  - **Convertible Reserved Instances:** This is similar to a flexible car lease, where you have the option to swap your car for a different model during your lease term. This gives you the flexibility to adjust to changing needs while still enjoying a long-term discount.

  - **Savings Plans:** This is like a mobile phone plan where you commit to spending a certain amount each month, and in return, you get a package of services that can vary in usage (e.g., data, voice minutes, texts). The flexibility comes from being able to use different services without having to commit to a specific one.

  - **Spot Instances:** This is like bidding for a hotel room on a website like Priceline, where you can get a significant discount but the hotel has the right to cancel your reservation if they can sell it to someone else at a higher price. This is best for situations where you can tolerate interruptions.

  - **Dedicated Hosts:** This is like renting an entire apartment building. You have control over the whole building and can decide how to allocate the apartments (instances). This is suitable for organizations with specific compliance requirements or for situations where you have software that is licensed per physical server.

  - **Dedicated Instances:** This is like renting an apartment in a building. The apartment is yours, but you don't have control over the entire building. You may have neighbors (other instances) from your own family (account) living in the same building.

  - **Capacity Reservations:** This is like reserving a table at a restaurant. You reserve the table (capacity) for a specific time (in an Availability Zone), and you pay for it whether you show up or not. This is ideal for situations where you anticipate a high demand for a short period and need to ensure you have the necessary capacity.

## Security in EC2

### Security Groups and Classic Ports Overview

- Fundamental to network security in AWS.

  - Control inbound and outbound traffic to/from EC2 instances.
  - Only contain allow rules.
  - Rules can reference by IP or by another security group.

- Security Groups Deeper Dive

  - Act as a firewall on EC2 instances.
  - Regulate:
    - Access to ports
    - Authorized IP ranges (IPv4 and IPv6)
    - Control of inbound network (from others to the instance)
    - Control of outbound network (from the instance to others)

- Security Groups Diagram

  - Inbound rules determine if traffic from outside is allowed into the EC2 instance.
  - Outbound rules determine if the EC2 instance can send traffic to the outside.

- Important Points about Security Groups

  - Can be attached to multiple instances.
  - Locked down to a region / VPC combination.
  - Live "outside" the EC2 - if traffic is blocked, the EC2 instance won’t see it.
  - Good to maintain a separate security group for SSH access.
  - If the application is not accessible (times out), it's a security group issue.
  - If the application gives a “connection refused“ error, then it's an application error or it's not launched.
  - By default, all inbound traffic is blocked and all outbound traffic is authorized.

- Referencing other Security Groups
  - Security groups can reference other security groups in their rules.
  - This allows EC2 instances with the referenced security groups to communicate with each other, regardless of their IP addresses.
- Classic Ports to Know
  - 22 = SSH (Secure Shell) - log into a Linux instance.
  - 21 = FTP (File Transfer Protocol) – upload files into a file share.
  - 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH.
  - 80 = HTTP – access unsecured websites.
  - 443 = HTTPS – access secured websites.
  - 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance.

### EC2 Instance Roles Demo

1. **Connect to EC2 Instance:\*** Use EC2 Instance Connect or SSH. For the sake of simplicity, EC2 Instance Connect is used. The terminal shows `ec2-user@<private IP>`.

2. **Linux Commands:** Execute basic Linux commands like `ping Google` to see it's working.

3. **Clear the Screen:** Use `clear` to clean the terminal screen.

4. **Use AWS IAM Commands:** The Amazon Linux AMI comes with AWS CLI installed. Use it to run IAM commands. For example, aws `iam list users` may fail initially as it's unable to locate the credentials.

5. **Don't Use AWS Configure:** Never run `aws configure` and enter personal details (Access Key ID and Secret Access Key) onto an EC2 instance. It exposes the credentials to anyone who can connect to the instance, posing a significant security risk.

6. **Use IAM Roles:** IAM roles should be used to provide AWS credentials to EC2 instances. Example: A demo role for EC2 with an IAMReadOnlyAccess policy.

7. **Attach IAM Role to EC2 Instance:** This can be done from the EC2 console. Go to Instances > Actions > Security > Modify IAM Role > Select the role > Save.

8. **Verify Role Assignment:** Run `aws iam list users` again. If the IAM role is correctly attached, the command will execute successfully.

9. **Role Change Propagation Delay:** If you detach or attach a policy to a role, it may take some time to propagate the changes across AWS. If a command is run immediately after a policy change, it may fail initially but work after some time.

Key takeaway: **Use IAM roles to provide AWS credentials to EC2 instances, never input your personal Access Key ID and Secret Access Key directly onto an EC2 instance.**

## Access and Connectivity

Different ways of connecting to your cloud servers, specifically Amazon EC2 instances, for maintenance or other actions. Depending on the operating system of your local machine, there are various methods to achieve this:

- **Secure Shell (SSH):** This is a command line interface utility that can be used on Mac, Linux, and Windows 10 or later. SSH allows you to securely connect to your server.

- **Putty:** This is a free and open-source terminal emulator that provides similar functionalities as SSH. It can be used if you're on a version of Windows older than Windows 10.

- **EC2 Instance Connect:** This is a method that lets you use your web browser to connect to your EC2 instance. It's compatible with all operating systems and doesn't require the installation of any additional software. However, as of the assistant's knowledge cutoff in September 2021, it only works with Amazon Linux 2 instances.

### How to SSH using Linux or Mac

Providing a step-by-step guide on how to SSH into an EC2 instance from a Linux or Mac computer. Here are the main steps:

1. Make sure your PEM file (the key pair file you downloaded from AWS) doesn't have any spaces in its name.

2. Place the PEM file in a directory you like.

3. Navigate to your EC2 instance overview page and find your instance.

4. Copy the public IPv4 address of your instance for later use.

5. Check that your security group has a rule allowing SSH traffic (port 22) from anywhere (0.0.0.0/0).

6. Try to SSH into your instance using the command `ssh ec2-user@your-instance-ip`. The "ec2-user" is a preconfigured user on Amazon Linux 2 instances.

7. If authentication fails, add the reference to your PEM file to the command: `ssh -i EC2Tutorial.pem ec2-user@your-instance-ip`.

8. If you get an error about unprotected key files, change the permissions for the PEM file using the command `chmod 0400 EC2Tutorial.pem`.

9. Try the SSH command again. If successful, you will be logged into your EC2 instance.

Remember that if you stop and start your instance, the public IP can change, so you may need to update this in your SSH command. To exit the SSH session, you can either type `exit` or use the keyboard shortcut `Control + D`.

### EC2 Instance Connect

EC2 Instance Connect is a feature provided by Amazon Web Services (AWS) that allows for browser-based SSH sessions to EC2 instances, bypassing the need for traditional SSH clients and key management.

Here's a step-by-step summary:

1. **Access:** Go to your AWS EC2 dashboard and click on the instance you want to connect to. Click the "Connect" button at the top of the dashboard.

2. **Options:** There are multiple connection options, including traditional SSH. Choose EC2 Instance Connect for a browser-based SSH session.

3. **User Verification:** The default username is EC2 User. This is usually appropriate for Amazon instances, but it can be overridden if necessary. However, entering "EC2 User" is usually required for successful connection.

4. **SSH Key:** Unlike with traditional SSH, you do not need to provide an SSH key. When you connect, AWS automatically uploads a temporary SSH key for you.

5. **Connection:** Click "Connect" to open a new tab and establish the SSH session. You will be logged into your Amazon Linux 2 AMI, where you can run commands like "whoami" or "ping google.com".

6. **Browser-Based Session:** This feature allows you to interact with your instance directly in your browser, without the need for a separate command line interface like Terminal.

7. **Security Group Rules:** EC2 Instance Connect still relies on SSH behind the scenes. Therefore, if you remove the SSH inbound rule from your instance's security group, you will not be able to connect. To fix this, you need to add the SSH rule back in, for both IPV4 and IPV6 if necessary.

8. **Try to Connect Again:** Once the SSH rules are properly set, you should be able to connect to the instance with EC2 Instance Connect.

In summary, EC2 Instance Connect offers a convenient, browser-based SSH session, removing the need to manage SSH keys, but still requires port 22 to be open. It works out-of-the-box with Amazon Linux 2.

## Quiz: EC2 Fundamentals Quiz

- **EC2 Purchasing Options:** Amazon EC2 offers a range of purchasing options to cater to different workload requirements and usage patterns.

  - **Spot Instances** offer significant discounts but are less reliable because they can be interrupted by AWS at any time if the resources are needed elsewhere. They are suitable for flexible, non-critical jobs, but not recommended for critical tasks or databases.

  - **Reserved Instances** provide a significant discount compared to On-Demand and are suitable for predictable workloads or applications that require reserved capacity. They can be reserved for 1 or 3 years only.

  - **Dedicated Hosts** are physical servers dedicated for your use. They can help reduce costs by allowing you to use your existing server-bound software licenses. They also provide visibility into the number of sockets and physical cores on the underlying server, which is important for some software licensing models. This is the most expensive EC2 Purchasing Option available.

- **Security Groups:** Security Groups are virtual firewalls for your EC2 instances to control inbound and outbound traffic. They operate at the EC2 instance level and can be attached to multiple EC2 instances within the same AWS Region/VPC.

- **EC2 Instance Types:** Amazon EC2 provides a variety of instance types optimized to fit different use cases.

  - **Compute Optimized** instances are ideal for compute-bound applications that require high performance processors.

  - **Memory Optimized** instances are designed to deliver fast performance for workloads that process large data sets in memory. They are suitable for applications that use in-memory databases.

  - **Storage Optimized** instances are designed for workloads that require high, sequential read/write access to large data sets on local storage. They are suitable for high-frequency OLTP databases.

- **EC2 User Data:** User Data in Amazon EC2 is used to automate boot tasks, such as installing software packages or updating server software, when EC2 instances are launched.

- **Compliance and Licensing with AWS:** Some companies have strict compliance requirements or software with complicated licensing models. For these cases, AWS offers Dedicated Hosts, which provide physical servers for your exclusive use, allowing you to comply with these requirements and utilize your existing licenses.

- **AWS Migration:** AWS provides a range of services and options to help you migrate your applications, databases, and other workloads to the cloud, including dedicated hosts for compliance and licensing requirements.

[Previous: Accessing AWS](./accessing-aws.md) | [Back to main page](../README.md) | [Next: EC2 Instance Storage](./ec2-instance-storage.md)
