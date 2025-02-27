# EC2 Instance Storage

## Table of Contents

- [EC2 Instance Storage](#ec2-instance-storage)
  - [Table of Contents](#table-of-contents)
  - [Understanding Storage Options](#understanding-storage-options)
    - [EBS Overview](#ebs-overview)
    - [EBS Volume Types](#ebs-volume-types)
    - [EC2 Instance Store](#ec2-instance-store)
    - [Amazon EFS](#amazon-efs)
      - [Performance and Storage Classes](#performance-and-storage-classes)
      - [Storage Classes](#storage-classes)
      - [Example](#example)
    - [EFS vs EBS](#efs-vs-ebs)
      - [EBS (Elastic Block Storage)](#ebs-elastic-block-storage)
      - [EFS (Elastic File System)](#efs-elastic-file-system)
      - [Instance Store](#instance-store)
  - [Storage Operations](#storage-operations)
    - [EBS Snapshots](#ebs-snapshots)
    - [AMI Overview](#ami-overview)
    - [EBS Multi-Attach](#ebs-multi-attach)
  - [Quiz: EC2 Data Management Quiz](#quiz-ec2-data-management-quiz)

## Understanding Storage Options

### EBS Overview

**Description:** EBS (Elastic Block Storage) volumes are network drives that can be attached to EC2 instances while they run. They can only be mounted to one instance at a time and are bound to a specific availability zone (AZ). They are used to persist data even after instance termination.

**Analogy:** Think of them as "network USB sticks."

**Free Tier:** AWS provides 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month.

**Latency:** Since EBS volumes use the network to communicate with the instance, there might be slight latency.

**Switching Instances:** EBS volumes can be detached from one EC2 instance and attached to another one quickly.

**Availability Zones:** An EBS volume is locked to an AZ. For instance, a volume in us-east-1a can't be attached to us-east-1b. To move a volume across, you first need to snapshot it.

**Capacity:** EBS volumes have a provisioned capacity (size in GBs, and IOPS), and you get billed for all the provisioned capacity. You can increase the capacity over time.

**EBS - Delete on Termination Attribute:**

- **Description:** This attribute controls EBS behaviour when an EC2 instance is terminated.

- **Default Setting:** By default, the root EBS volume is deleted (attribute enabled). Any other attached EBS volume is not deleted (attribute disabled).

- **Control:** This can be controlled through AWS console or AWS CLI.

- **Use Case:** Preserve root volume when instance is terminated. To achieve this, disable delete on termination for the root volume.

### EBS Volume Types

**1. General Purpose SSD (gp2/gp3):** Balances price and performance for a wide variety of workloads.

- Use Cases: System boot volumes, virtual desktops, development and test environments.
- Size: 1 GiB - 16 TiB.
- gp3: Baseline of 3,000 IOPS and 125 MiB/s throughput. Can independently increase IOPS up to 16,000 and throughput up to 1,000 MiB/s.
- gp2: Small volumes can burst to 3,000 IOPS. Size of volume and IOPS are linked, max IOPS is 16,000. At 5,334 GB, IOPS is maxed out.

**2. Provisioned IOPS SSD (io1/io2):** For mission-critical low-latency or high-throughput workloads.

- Use Cases: Critical business applications with sustained IOPS performance, or applications that need more than 16,000 IOPS. Great for database workloads.
- Size: 4 GiB - 16 TiB.
- io2: More durability and more IOPS per GiB (at the same price as io1).
- io2 Block Express: 4 GiB – 64 TiB. Sub-millisecond latency. Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1. Supports EBS Multi-attach.
- Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other instances.

**3. Throughput Optimized HDD (st1):** Low-cost HDD volume designed for frequently accessed, throughput-intensive workloads.

- Use Cases: Big data, data warehouses, log processing.
- Size: 125 GiB - 16 TiB.
- Max throughput: 500 MiB/s. Max IOPS: 500.

**4. Cold HDD (sc1):** Lowest-cost HDD volume designed for less frequently accessed workloads.

- Use Cases: Infrequently accessed data, scenarios where lowest cost is important.
- Size: 125 GiB - 16 TiB.
- Max throughput: 250 MiB/s. Max IOPS: 250.

Note: Only gp2/gp3 and io1/io2 can be used as boot volumes. For over 32,000 IOPS, Nitro EC2 with io1 or io2 is needed.

### EC2 Instance Store

**1. EC2 Instance Store:** This is a type of storage available on Amazon EC2 instances that is physically connected to the server hosting the instance. It's designed to provide high-performance I/O operations.

**2. Use Cases:** EC2 Instance Stores are best suited for temporary data storage such as buffers, caches, and scratch data. They're not recommended for long-term storage due to their ephemeral nature.

**3. Performance:** EC2 Instance Stores provide significantly higher performance than EBS volumes. For instance, some EC2 instances with Instance Stores can reach up to 3.3 million Read IOPS and 1.4 million Write IOPS, compared to a maximum of 32,000 IOPS with EBS volumes.

**4. Ephemeral Storage:** If an EC2 instance with an Instance Store is stopped or terminated, the data stored on the Instance Store will be lost. This is why they are referred to as ephemeral storage.

**5. Responsibility for Data:** It's the user's responsibility to back up and replicate data stored on an EC2 Instance Store, particularly because there's a risk of data loss if the underlying server hardware fails.

Remember, when you see high-performance hardware-attached storage for EC2 instances, it's likely referring to EC2 Instance Stores.

### Amazon EFS

- Amazon EFS is a managed Network File System (NFS) that can be mounted on many EC2 instances across different Availability Zones (AZ).
- It is highly available, scalable, and more expensive (about 3x) than a gp2 EBS volume. You pay per use and don't need to provision capacity in advance.
- The system is secured with a security group and is only compatible with Linux-based AMI, not Windows.
  Encryption at rest can be enabled using KMS.
- Content management, web serving, data sharing, and WordPress applications.

#### Performance and Storage Classes

- EFS supports thousands of concurrent NFS clients and offers over 10 GB/s throughput, scaling automatically to Petabyte-scale.
- **_Performance Mode:_** Set at EFS creation time. Two options available:
  - General Purpose (default): Suitable for latency-sensitive use cases like web servers, CMS, etc.
  - Max I/O: Higher latency, higher throughput, and highly parallel. Suitable for big data applications or media processing.
- **_Throughput Mode:_** Various options available.
  - Bursting: 1TB = 50MiB/s + burst of up to 100MiB/s.
  - Provisioned: Allows you to set your throughput regardless of storage size.
  - Elastic: Automatically scales throughput based on your workloads, up to 3GiB/s for reads and 1GiB/s for writes. Ideal for unpredictable workloads.

#### Storage Classes

- Storage Tiers (lifecycle management feature) allows moving files to a different tier after N days.
  - Standard: For frequently accessed files.
  - Infrequent Access (EFS-IA): Lower cost to store, but cost to retrieve files. Enabled using a Lifecycle Policy.
- Availability and Durability:
  - Standard: Multi-AZ, suitable for production environments.
  - One Zone: Single AZ, suitable for development environments, backups enabled by default, and compatible with Infrequent Access (EFS One Zone-IA), offers over 90% in cost savings.

#### Example

- EC2 instances in us-east-1a, us-east-1b, and us-east-1c Availability Zones can all connect to the same EFS file system.
- If a file on EFS Standard hasn't been accessed for more than 60 days, a set lifecycle policy will move it to EFS-IA, lowering storage cost.

### EFS vs EBS

#### EBS (Elastic Block Storage)

- Attached to one instance at a time, except for multi-attach feature in io1/io2 volume types.
- Locked at the Availability Zone (AZ) level. An EBS volume in AZ1 cannot be attached to an instance in AZ2.
- For gp2 volume types, IO increases with disk size. For io1 volumes, IO can be increased independently.
- To migrate an EBS volume across AZs, take a snapshot and restore it in the target AZ.
- EBS backups use IO and should not be run when the application is handling high traffic to avoid performance impact.
- Root EBS volumes of instances are terminated by default if the EC2 instance is terminated (this can be disabled).

#### EFS (Elastic File System)

- Can be attached to hundreds of instances across different AZs.
- One EFS file system can have different mount targets in different AZs, allowing multiple instances to share the file system.
- Particularly useful for applications like WordPress.
- Only compatible with Linux instances due to the use of the POSIX system.
- EFS is more expensive than EBS, but cost savings can be achieved using the EFS-IA feature.

#### Instance Store

- Physically attached to the EC2 instance, so if the instance is lost, the storage is also lost.

## Storage Operations

### EBS Snapshots

- An EBS Snapshot is a backup of your EBS volume at a specific point in time.
- It's not necessary to detach the EBS volume from your EC2 instance to create a snapshot, but it's recommended.
- These snapshots can be copied across different Availability Zones or Regions.
- Example: You can create a snapshot of an EBS volume from an EC2 instance in US-EAST-1A and restore it to an EC2 instance in US-EAST-1B.
- EBS Snapshot Archive:
  - This feature allows you to move the snapshot to an "archive tier" which is up to 75% cheaper.
  - Restoring the archive can take anywhere from 24 to 72 hours, so it's not immediate.
- Recycle Bin for EBS Snapshots:
  - If you delete your EBS snapshots, they're not permanently deleted but moved to a Recycle Bin, allowing for recovery from accidental deletions.
  - You can set the retention period for your Recycle Bin anywhere between one day to one year.
- Fast Snapshot Restore (FSR):
  - This feature forces a full initialization of your snapshot to eliminate latency on the first use, which is useful for large snapshots that need to be initialized quickly.
  - However, this feature can be expensive, so careful consideration is needed before using it.

### AMI Overview

- **What is an AMI?** An Amazon Machine Image (AMI) is a template that contains a software configuration (operating system, application server, and applications) required to launch an Amazon EC2 instance. It's a customizable version of an EC2 instance.

- **Benefits of AMI:** With AMI, you achieve faster boot and configuration time as all your required software comes pre-packaged.

- **Types of AMI:** You can launch EC2 instances from three types of AMIs:

  - **Public AMI:** Provided by AWS (e.g., Amazon Linux 2 AMI).
  - **Your own AMI:** These are custom AMIs that you make and maintain yourself.
  - **AWS Marketplace AMI:** These are AMIs made and potentially sold by someone else, often vendors or businesses.

- AMI Across Regions: AMIs are built for a specific region but can be copied across regions to leverage AWS's global infrastructure.

- **AMI Process**

  - **Customize EC2 Instance:** Start an EC2 instance and tailor it to your needs.

  - **Stop the Instance:** This step ensures data integrity before creating the AMI.

  - **Build an AMI:** This step creates your AMI and will also generate EBS (Elastic Block Store) snapshots in the background.

  - **Launch Instances from AMI:** You can then launch other instances using the AMI you created.

  - **Example of Use:** For instance, you can launch an EC2 instance in US-EAST-1A, customize it, create an AMI from it, and then launch a new instance from that AMI in US-EAST-1B, effectively duplicating your EC2 instance.

### EBS Multi-Attach

- **Multi-Attach Feature:** It allows the same EBS volume to be attached to multiple EC2 instances within the same availability zone.

- **Volume Type:** This feature is available only for io1 and io2 family of EBS volumes.

- **Permissions:** Each attached EC2 instance has full read and write permissions, allowing simultaneous read and write operations.

- **Use Cases:** It enhances application availability for clustered Linux applications, such as Teradata. It's also useful for applications that need to manage concurrent write operations.

- **Availability Zone (AZ) Limitation:** Multi-Attach can't be used to attach an EBS volume from one AZ to another. It's restricted to a specific AZ.

- **Instance Limitation:** A maximum of 16 EC2 instances can be attached to the same volume at a time.

- **File System Requirement:** To utilize this feature, a cluster-aware file system must be used. Traditional file systems like XFS or EXT4 are not suitable.

## Quiz: EC2 Data Management Quiz

[Previous: EC2 Fundamentals](./ec2-fundamentals.md) | [Back to main page](../README.md) | [Next:](./)
