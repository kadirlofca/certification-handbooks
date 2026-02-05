# AWS Certified Solutions Architect - Associate Handbook

## Introduction

Welcome to the comprehensive handbook for the AWS Certified Solutions Architect - Associate exam. This guide is designed to take you from the basics to exam readiness, covering all four domains of the exam guide.

The content is structured to provide deep understanding, practical application, and exam-focused tips.

### How to Use This Handbook (40-Hour Study Plan)

Reading this text alone will take only a few hours. However, passing the exam requires **muscle memory** and deep familiarity with the AWS Console. This 40-hour plan assumes you are **pausing to build** what you read.

**The "Active Learning" Strategy:**
1.  **Read (20%):** Understand the concept.
2.  **Build (50%):** Log into the AWS Console and create the resource. Click through every tab. Read the "Info" tooltips.
3.  **Break (20%):** Intentionally misconfigure it (e.g., remove a security group rule) and observe the error.
4.  **Fix (10%):** Resolve the issue.

**Time Allocation:**
*   **Domain 1 (Security):** 10 Hours (Focus: IAM Policies, VPC Routing tables, Security Group vs NACL troubleshooting).
*   **Domain 2 (Resiliency):** 12 Hours (Focus: Setting up Auto Scaling from scratch, forcing Multi-AZ failovers).
*   **Domain 3 (Performance):** 10 Hours (Focus: Comparing EBS volume types, setting up CloudFront).
*   **Domain 4 (Cost Optimization):** 8 Hours (Focus: Cost Explorer analysis, spotting unused resources).

### ⚠️ Important Cost Warning
Most exercises in this handbook are designed to fit within the **AWS Free Tier** for new accounts. However, some resources (e.g., NAT Gateways, KMS Keys, Route 53 zones) may incur small costs if left running.
*   **Rule #1:** Delete resources immediately after finishing the assignment.
*   **Rule #2:** Check your **Billing Dashboard** daily.
*   **Rule #3:** If you are unsure about a cost, check the [AWS Free Tier page](https://aws.amazon.com/free/).

---

## The CloudMart Scenario

Throughout this handbook, you will act as a Solutions Architect for **CloudMart**, a fictional e-commerce startup. CloudMart is migrating from on-premises servers to AWS. You will build their infrastructure from scratch, layer by layer, starting with security and networking, then moving to resilience, performance, and cost optimization.

By the end of this course, you will have built a fully functional, production-ready environment for CloudMart.

## Prerequisites

To complete the assignments, you need:
1.  **An AWS Account:** (Free Tier recommended).
2.  **A Credit Card:** Required for account creation (you won't be charged if you follow the Free Tier limits, but accidents happen—see the Cost Warning).
3.  **Terminal Access:** A terminal with SSH capability (Mac/Linux terminal, or PowerShell/PuTTY on Windows).

---

# Domain 1: Design Secure Architectures

Security is a primary consideration in AWS. It involves securing access to resources, securing workloads/applications, and ensuring data privacy and compliance.

## 1.1 Design Secure Access to AWS Resources

This section focuses on Identity and Access Management (IAM), the Shared Responsibility Model, and securing access across single and multi-account environments.

### 1.1.1 AWS Identity and Access Management (IAM)

IAM (Identity and Access Management) is a global service used to manage access to AWS (Amazon Web Services) services and resources securely.

**Core Components:**
1.  **Users:** Represent a person or service. By default, new users have **no permissions**.
2.  **Groups:** A collection of users. Permissions attached to a group apply to all users in that group. Best practice: Attach policies to groups, not individual users.
3.  **Roles:** An identity that you can create in your account that has specific permissions. Roles are **assumed** by users, applications, or AWS services. They provide temporary security credentials.
4.  **Policies:** JSON documents that define permissions.

**Types of Policies:**
*   **Identity-Based Policies:** Attached to a User, Group, or Role. Defines *what that identity can do*.
*   **Resource-Based Policies:** Attached to a resource (e.g., S3 Bucket Policy, SQS Queue Policy, KMS Key Policy). Defines *who can access this resource*.
    *   *Note:* The "Principal" element is required in Resource-based policies to specify who is allowed.
*   **Permissions Boundaries:** An advanced feature to set the maximum permissions an identity-based policy can grant to an IAM entity.

**Decision Logic:**
When AWS evaluates a request:
1.  By default, all requests are **Implicitly Denied**.
2.  An **Explicit Allow** in a policy overrides the default deny.
3.  An **Explicit Deny** in *any* policy overrides *any* allow.

### 1.1.2 Best Practices for Account Security

1.  **Secure the Root User:**
    *   The Root User has unlimited access.
    *   **Never** use the root user for daily tasks.
    *   Enable **MFA (Multi-Factor Authentication)** on the root user immediately.
    *   Delete root access keys if they exist.
2.  **Principle of Least Privilege:** Grant only the permissions required to perform a task and no more.
3.  **Enable MFA (Multi-Factor Authentication):** Enforce MFA (Multi-Factor Authentication) for all users, especially privileged ones.

### 1.1.3 Federation and SSO

*   **AWS IAM Identity Center (formerly AWS SSO (Single Sign-On)):** The recommended service for managing human access to AWS (Amazon Web Services). It supports connecting to external identity providers (IdPs) like Active Directory, Okta, or Ping Identity.
*   **SAML (Security Assertion Markup Language) 2.0:** Standard used for federation. Users authenticate with their corporate IdP (Identity Provider) and assume an IAM (Identity and Access Management) Role in AWS.

### 1.1.4 Multi-Account Strategy

Using a single account for everything is a security risk and unmanageable at scale.

*   **AWS Organizations:** Central governance and management of multiple AWS accounts.
    *   **Consolidated Billing:** Pay for all accounts via a single payment method.
    *   **Service Control Policies (SCPs):** A type of organization policy that manages permissions in your organization. SCPs (Service Control Policies) offer central control over the maximum available permissions for all accounts in your organization. *SCPs (Service Control Policies) do not grant permissions; they filter them.*
*   **AWS Control Tower:** A service that effectively automates the set-up of a multi-account environment using AWS Organizations and best practices (Landing Zone).

---

### **Mini-Project 1: CloudMart's Identity Foundation (Estimated Time: 1.5 - 2 Hours)**
*Goal: Set up the secure environment for CloudMart's development team.*

1.  **Root Security:** Log in as Root. Enable MFA. **Create a Billing Alarm** in CloudWatch (Region: us-east-1) to alert you if spending exceeds $5.
2.  **IAM Groups Structure:** CloudMart needs role separation. Create three groups:
    *   `CloudMart-Admins` (AdministratorAccess)
    *   `CloudMart-Devs` (PowerUserAccess)
    *   `CloudMart-Auditors` (ReadOnlyAccess)
3.  **User Creation:**
    *   Create a user `Alice` (Solutions Architect) and add to `CloudMart-Admins`.
    *   Create a user `Bob` (Developer) and add to `CloudMart-Devs`.
4.  **Policy Simulation:**
    *   Log in as `Bob` (the Developer).
    *   Try to create a new IAM User (Should fail).
    *   Try to launch an EC2 instance (Should succeed).
    *   Try to delete a CloudWatch log group (Should succeed).
5.  **Password Policy:** Set a strict password policy (12 chars, rotation every 90 days) to comply with CloudMart's security policy.
6.  **Cleanup:** You can keep these users for the next assignment, or delete them to save clutter.

---

## 1.2 Design Secure Workloads and Applications

This section covers network security (VPC), application security, and specific security services.

### 1.2.1 VPC Security Layers

Security in a Virtual Private Cloud (VPC) works in layers.

| Feature | Security Group (SG) | Network ACL (NACL) |
| :--- | :--- | :--- |
| **Level** | Instance Level | Subnet Level |
| **State** | **Stateful:** Return traffic is automatically allowed. | **Stateless:** Return traffic must be explicitly allowed. |
| **Rules** | Allow rules only. (Implicit deny at the end). | Allow and Deny rules. |
| **Order** | All rules evaluated before decision. | Processed in number order. |
| **Use Case** | First line of defense for EC2 (Elastic Compute Cloud)/RDS (Relational Database Service)/ENI (Elastic Network Interface). | Network segmentation, blocking specific IPs (Internet Protocols). |

### 1.2.2 Network Segmentation

*   **Public Subnet:** Has a route to an Internet Gateway (IGW). Used for resources that must face the internet (e.g., Load Balancers, Bastion Hosts).
*   **Private Subnet:** No direct route to the Internet. Used for backend servers, databases, and application logic.
    *   **NAT (Network Address Translation) Gateway:** Allows instances in a private subnet to access the internet (e.g., for updates) but prevents the internet from initiating connections to instances. Deployed in a *Public Subnet*.

### 1.2.3 Secure Access to AWS Services

*   **VPC Endpoints:** Allow you to connect your VPC to AWS services privately without leaving the Amazon network (no Internet Gateway required).
    *   **Gateway Endpoints:** Used for **S3 (Simple Storage Service)** and **DynamoDB**. Defined in route tables. Free of charge.
    *   **Interface Endpoints (PrivateLink):** Used for most other services (e.g., SNS (Simple Notification Service), SQS (Simple Queue Service), EC2 (Elastic Compute Cloud)). Creates an ENI (Elastic Network Interface) in your subnet. Costs money.

### 1.2.4 Application Security Services

*   **AWS WAF (Web Application Firewall):** Protects web apps from common exploits (SQL (Structured Query Language) injection, XSS (Cross-Site Scripting)). Attaches to ALB (Application Load Balancer), API (Application Programming Interface) Gateway, and CloudFront.
*   **AWS Shield:** Managed DDoS (Distributed Denial of Service) protection.
    *   **Standard:** Free, always on (L3/L4 (Layer 3/Layer 4) protection).
    *   **Advanced:** Paid, adds L7 (Layer 7) protection, access to DDoS (Distributed Denial of Service) response team (DRT), and cost protection against spikes.
*   **AWS Inspector:** Automated security assessment service that helps improve the security and compliance of applications deployed on AWS (EC2 (Elastic Compute Cloud) vulnerability scanning).
*   **Amazon GuardDuty:** Intelligent threat detection service. Uses ML (Machine Learning) to analyze CloudTrail logs, VPC (Virtual Private Cloud) Flow Logs, and DNS (Domain Name System) logs to detect anomalies (e.g., crypto mining, unauthorized access).

### 1.2.5 Secrets Management

*   **AWS Secrets Manager:** Designed specifically for credentials (database passwords, API keys). **Supports automatic rotation** of secrets (e.g., changing an RDS password automatically).
*   **AWS Systems Manager Parameter Store:** Store hierarchal configuration data and secrets. Can store secure strings, but **does not** support automatic rotation natively without custom Lambda functions.

---

### **Mini-Project 2: CloudMart's Network Backbone (Estimated Time: 2 - 3 Hours)**
*Goal: Build the VPC where CloudMart's application will live.*

1.  **VPC Scratch Build:** Create a VPC named `CloudMart-VPC` with CIDR `10.0.0.0/16`. Do **not** use the wizard.
2.  **Subnets:** CloudMart requires network segmentation.
    *   Create `CloudMart-Public-Subnet` (10.0.1.0/24).
    *   Create `CloudMart-Private-Subnet` (10.0.2.0/24).
3.  **Routing:**
    *   Create an Internet Gateway and attach it to `CloudMart-VPC`.
    *   Create a public route table (0.0.0.0/0 -> IGW) and associate it with `CloudMart-Public-Subnet`.
4.  **EC2 Launch (Simulating the App):**
    *   Launch `CloudMart-Bastion` in the Public Subnet (Auto-assign Public IP: Enable). Security Group: Allow SSH from *Your IP*.
    *   Launch `CloudMart-App-Server` in the Private Subnet (No Public IP). Security Group: Allow SSH from the `CloudMart-Bastion` Security Group ID.
5.  **Connectivity Test:**
    *   SSH into `CloudMart-Bastion`.
    *   From there, SSH into `CloudMart-App-Server` (Use SSH Agent Forwarding).
    *   *Result:* You have securely accessed a private resource via a Bastion host.
6.  **NACL Experiment:** Go to the VPC NACL. Add a rule to DENY port 22 from your specific IP. Observe how your session drops immediately (stateless). Remove the rule.
7.  **Cleanup:** Terminate the EC2 instances. **Keep the VPC and Subnets** for Mini-Project 3.

---

## 1.3 Determine Appropriate Data Security Controls

Focuses on encryption, data privacy, and compliance.

### 1.3.1 Encryption Fundamentals

*   **Encryption at Rest:** Protecting data stored on disk (EBS (Elastic Block Store), S3 (Simple Storage Service), RDS (Relational Database Service)).
*   **Encryption in Transit:** Protecting data moving between endpoints (HTTPS (Hypertext Transfer Protocol Secure)/TLS (Transport Layer Security)).

### 1.3.2 AWS Key Management Service (KMS)

KMS (Key Management Service) is a managed service for creating and managing cryptographic keys.

*   **CMK (Customer Master Key):**
    *   **AWS Managed CMK:** Free, created by AWS services (e.g., `aws/s3`). You cannot manage policies or rotate explicitly.
    *   **Customer Managed CMK:** You create, own, and manage. You control the key policy and rotation (automatic rotation every year).
*   **Symmetric Keys:** Same key for encryption and decryption. Used for most AWS services.
*   **Asymmetric Keys:** Public/Private key pair. Used for signing and verification or encryption outside AWS.
*   **CloudHSM:** dedicated hardware security module. Use if you need *single-tenant* hardware or specific compliance (FIPS 140-2 Level 3). KMS is FIPS 140-2 Level 2.

### 1.3.3 S3 Data Security

*   **Server-Side Encryption (SSE):**
    *   **SSE-S3 (Server-Side Encryption with S3 Managed Keys):** Keys handled/managed by S3 (Simple Storage Service) (AES-256).
    *   **SSE-KMS (Server-Side Encryption with KMS Managed Keys):** Keys handled by KMS (Key Management Service). Provides audit trail in CloudTrail and user access control.
    *   **SSE-C (Server-Side Encryption with Customer Provided Keys):** Customer provided keys. You manage the key; S3 (Simple Storage Service) does the encryption/decryption.
*   **Client-Side Encryption:** You encrypt data *before* sending it to AWS.
*   **S3 Block Public Access:** A setting at the account or bucket level to prevent accidental public exposure. Always enabled by default on new buckets.

### 1.3.4 Compliance and Discovery

*   **Amazon Macie:** Uses Machine Learning (ML) to discover and protect sensitive data (PII (Personally Identifiable Information)) in **S3 (Simple Storage Service)**.
*   **AWS Artifact:** Central resource for compliance-related information (download AWS ISO certifications, SOC (System and Organization Controls) reports).
*   **AWS Certificate Manager (ACM):** Provision, manage, and deploy SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificates for use with AWS services (ALB (Application Load Balancer), CloudFront).

---

### **Mini-Project 3: CloudMart's Encrypted Data Lake (Estimated Time: 1.5 Hours)**
*Goal: Ensure CloudMart's customer data is encrypted at rest using strict policies.*

1.  **KMS Management:** CloudMart requires a dedicated key. Create a Customer Managed Key (CMK) in KMS named `CloudMart-Data-Key`.
2.  **Bucket Policy:** Create an S3 Bucket `cloudmart-customer-data-[your-initials]`. Add a **Bucket Policy** that explicitly denies `s3:PutObject` if the request does not include `x-amz-server-side-encryption`.
3.  **Testing:**
    *   Try to upload a file `customer_list.csv` via console *without* specifying encryption. It should fail.
    *   Upload the file again, specifying the `CloudMart-Data-Key` for encryption. It should succeed.
4.  **Audit:** Enable **AWS CloudTrail**. Perform an upload. Go to CloudTrail Event History and find the `PutObject` event. Verify the `encryption` details show usage of the CloudMart key.
5.  **Cleanup:** Empty and delete the S3 Bucket. Schedule the KMS Key for deletion (minimum waiting period is 7 days).

---

# Domain 2: Design Resilient Architectures

Resiliency is the ability of a workload to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.

## 2.1 Design Scalable and Loosely Coupled Architectures

### 2.1.1 Scaling Concepts

*   **Scalability:** The ability of a system to accommodate larger loads.
*   **Vertical Scaling (Scale Up):** Increasing the size of the instance (e.g., t2.micro to t2.large).
    *   *Limit:* Hardware limits. Often requires downtime to resize.
*   **Horizontal Scaling (Scale Out):** Adding more instances to the pool.
    *   *Benefit:* No theoretical limit. Requires stateless applications.
*   **Elasticity:** The ability to automatically scale out and scale in based on demand.

### 2.1.2 Compute Scaling

*   **Amazon EC2 (Elastic Compute Cloud) Auto Scaling:** Ensures you have the correct number of EC2 (Elastic Compute Cloud) instances available to handle the load for your application.
    *   **Launch Template:** Contains the configuration (AMI (Amazon Machine Image), Instance Type, Key Pair, Security Group). *Replaces Launch Configurations.*
    *   **Scaling Policies:**
        *   *Target Tracking:* "Keep CPU at 50%." (Simplest to set up).
        *   *Simple/Step Scaling:* "Add 2 units when alarm breaches."
        *   *Scheduled Scaling:* "Scale out at 9 AM on Monday."
*   **Load Balancing (ELB (Elastic Load Balancing)):** Distributes incoming application traffic across multiple targets (EC2 (Elastic Compute Cloud) instances, containers, IP (Internet Protocol) addresses).
    *   **Application Load Balancer (ALB):** Layer 7 (HTTP (Hypertext Transfer Protocol)/HTTPS (Hypertext Transfer Protocol Secure)). Supports path-based routing, host-based routing, and containers.
    *   **Network Load Balancer (NLB):** Layer 4 (TCP (Transmission Control Protocol)/UDP (User Datagram Protocol)). Ultra-high performance, static IP (Internet Protocol) addresses.
    *   **Gateway Load Balancer (GWLB):** Used for deploying third-party virtual appliances (firewalls).

### 2.1.3 Decoupling Architectures

Decoupling ensures that components of an application can scale and fail independently.

*   **Amazon SQS (Simple Queue Service):** Fully managed message queuing service.
    *   **Standard Queue:** Unlimited throughput, at-least-once delivery, best-effort ordering.
    *   **FIFO (First-In-First-Out) Queue:** Preserves order (FIFO (First-In-First-Out)), exactly-once processing, limited throughput (300/s without batching).
    *   *Use Case:* Buffer requests between a web server and a backend worker.
*   **Amazon SNS (Simple Notification Service):** Pub/Sub (Publish/Subscribe) messaging. One message sent to a topic is delivered to multiple subscribers (Email, SQS (Simple Queue Service), Lambda, HTTP (Hypertext Transfer Protocol)).
*   **Amazon EventBridge:** Serverless event bus. Connects applications using data from your own apps, SaaS (Software as a Service) apps, and AWS services.
*   **Amazon API (Application Programming Interface) Gateway:** "Front door" for applications to access data, business logic, or functionality from your backend services.

### 2.1.4 Serverless and Microservices

*   **AWS Lambda:** Run code without provisioning or managing servers.
    *   *Scaling:* Scales automatically by running code in response to each trigger.
    *   *Event-Driven:* Triggered by S3 (Simple Storage Service), DynamoDB, API (Application Programming Interface) Gateway, etc.
*   **Amazon ECS (Elastic Container Service)/EKS (Elastic Kubernetes Service):** Container orchestration.
    *   **Fargate:** Serverless compute engine for containers. Removes the need to provision and manage servers (EC2 (Elastic Compute Cloud) instances) for your cluster.

---

### **Mini-Project 4: CloudMart's Elastic Web Farm (Estimated Time: 2.5 Hours)**
*Goal: Deploy CloudMart's frontend in the VPC created in Project 2 and make it scalable.*

1.  **Launch Template:** Create `CloudMart-Web-Template` using Amazon Linux 2.
    *   User Data: `#!/bin/bash` followed by commands to install `httpd` and `stress` (`sudo amazon-linux-extras install epel -y && sudo yum install stress -y`).
    *   Security Group: Allow HTTP (80) from anywhere.
2.  **ALB Setup:** Create an Application Load Balancer `CloudMart-ALB` in the **Public Subnets** of `CloudMart-VPC` (from Project 2).
3.  **ASG Setup:** Create an Auto Scaling Group `CloudMart-ASG` using the template.
    *   *Lab Adjustment:* Deploy ASG into **CloudMart-Public-Subnet** to avoid NAT Gateway costs for this lab. (In production, this would go in Private).
    *   Min: 1, Desired: 2, Max: 4.
4.  **Scaling Policy:** Add a **Target Tracking Policy** to keep Average CPU at 50%.
5.  **Stress Test:**
    *   SSH into one of the instances (via `CloudMart-Bastion` or direct public IP). Run `stress --cpu 1 --timeout 300`.
    *   Watch **CloudWatch Metrics** -> EC2 -> CPUUtilization spike.
    *   Observe the ASG launching a new instance to handle the load.
6.  **Cleanup:** Delete the Auto Scaling Group, Load Balancer, Target Group, and Launch Template.

---

## 2.2 Design Highly Available and Fault-Tolerant Architectures

*   **High Availability (HA):** System is accessible most of the time (e.g., "four nines" 99.99%). Minimal downtime.
*   **Fault Tolerance:** System continues to function with zero downtime even if a component fails. (More expensive).
*   **Disaster Recovery (DR):** Preparing for and recovering from catastrophic events.

### 2.2.1 AWS Global Infrastructure

*   **Regions:** Physical location around the world (e.g., us-east-1).
*   **Availability Zones (AZ):** One or more discrete data centers with redundant power, networking, and connectivity in an AWS Region.
*   **Edge Locations:** Used by CloudFront and Route 53 to cache content closer to users.

### 2.2.2 Database Resiliency

*   **Amazon RDS (Relational Database Service) Multi-AZ (Availability Zone):**
    *   *Purpose:* High Availability (HA) / Disaster Recovery (DR).
    *   *Mechanism:* Synchronous replication to a standby instance in a different AZ (Availability Zone).
    *   *Failover:* Automatic update of DNS (Domain Name System) endpoint if primary fails.
*   **Amazon RDS (Relational Database Service) Read Replicas:**
    *   *Purpose:* Scalability (Offload read traffic).
    *   *Mechanism:* Asynchronous replication.
    *   *Note:* Can be promoted to a standalone DB (DR strategy).
*   **Amazon Aurora:**
    *   *Storage:* Data is replicated 6 times across 3 AZs (Availability Zones).
    *   *Global Database:* Replicate data to another Region for DR (Disaster Recovery) (latency < 1s).
*   **DynamoDB Global Tables:** Multi-region, multi-active replication.

### 2.2.3 Routing and Failover (Route 53)

Amazon Route 53 is a highly available and scalable cloud DNS (Domain Name System) web service.

*   **Simple Routing:** One record, multiple IPs (Internet Protocol) (random). No health checks.
*   **Weighted Routing:** Split traffic based on assigned weights (e.g., 80% to V1, 20% to V2).
*   **Latency-Based Routing:** Route to the region with the lowest latency for the user.
*   **Failover Routing:** Active-Passive configuration. If Primary health check fails, route to Secondary (DR (Disaster Recovery) site).
*   **Geolocation Routing:** Route based on user location (e.g., users in France go to French servers).
*   **AWS Global Accelerator:** Uses AWS global network to improve availability and performance. Provides static IPs (Internet Protocols). Good for non-HTTP (Hypertext Transfer Protocol) traffic (gaming, IoT (Internet of Things)).

### 2.2.4 Disaster Recovery Strategies

Ordered from cheapest/slowest to most expensive/fastest:

1.  **Backup and Restore:** Data is backed up to S3. In disaster, restore data and rebuild infra. (High RPO/RTO).
2.  **Pilot Light:** Critical core elements (DB) are configured and running (usually small). App servers are off/not provisioned. In disaster, turn on app servers and scale up DB.
3.  **Warm Standby:** A scaled-down version of the fully functional environment is always running. In disaster, scale up everything.
4.  **Multi-Site Active/Active:** Full capacity running in multiple regions. Traffic flows to both. (Zero RPO/RTO possible).

*   **RPO (Recovery Point Objective):** How much data loss is acceptable? (Time).
*   **RTO (Recovery Time Objective):** How quickly must the system be back online? (Time).

---

### **Mini-Project 5: CloudMart's Resilient Database (Estimated Time: 2 Hours)**
*Goal: Ensure CloudMart's product catalog stays online during a failure.*

1.  **Primary DB:** Launch an RDS MySQL instance `CloudMart-Product-DB` in `CloudMart-VPC`.
    *   Select **Dev/Test** (Single AZ) to start.
    *   Security Group: Allow port 3306 from anywhere (for this lab) or from your specific IP.
2.  **Read Replica:** CloudMart's analytics team needs read access. Create a Read Replica in a *different* AZ.
3.  **Connection Test:**
    *   Connect to `CloudMart-Product-DB` using a SQL client (e.g., DBeaver on your laptop, or a temp EC2 instance).
    *   Insert a dummy product: `CREATE TABLE products (name VARCHAR(20)); INSERT INTO products VALUES ('Sneaker');`
    *   Verify you can read it from the Read Replica, but cannot write to the Replica.
4.  **Simulate Disaster:**
    *   Assume the Primary AZ fails. Promote the Read Replica to Standalone.
    *   Verify the "Sneaker" data exists in the new primary.
5.  **Cleanup:** Delete both databases (Snapshot: No).

---

# Domain 3: Design High-Performing Architectures

Performance efficiency is about using computing resources efficiently to meet system requirements and to maintain that efficiency as demand changes and technologies evolve.

## 3.1 Determine High-Performing and/or Scalable Storage Solutions

### 3.1.1 Storage Types and Use Cases

| Storage Type | Service | Characteristics | Use Case |
| :--- | :--- | :--- | :--- |
| **Block** | **Amazon EBS (Elastic Block Store)** | Low latency, persistent, mountable to one EC2 (Elastic Compute Cloud) (mostly). | Operating systems, databases (high IOPS (Input/Output Operations Per Second)). |
| | **Instance Store** | Ephemeral (temporary), physically attached to host. Highest performance. | Cache, buffers, temporary content. Data is lost on stop/terminate. |
| **File** | **Amazon EFS (Elastic File System)** | Shared file system (NFS (Network File System)), Linux only, regional, elastic. | CMS (Content Management System), shared repositories, lift-and-shift web apps. |
| | **Amazon FSx for Windows** | SMB (Server Message Block) protocol, Windows native. | Windows file shares, AD (Active Directory) integration. |
| | **Amazon FSx for Lustre** | HPC (High-Performance Computing). | Machine learning, video processing. |
| **Object** | **Amazon S3 (Simple Storage Service)** | Flat structure (key-value), infinite scale, HTTP (Hypertext Transfer Protocol) access. | Static websites, backups, media hosting, data lakes. |

### 3.1.2 S3 Performance

*   **Multipart Upload:** Recommended for files > 100MB. Required for files > 5GB. Parallelizes uploads for speed.
*   **S3 Transfer Acceleration:** Uses AWS Edge Locations to speed up uploads over the public internet.

### 3.1.3 EBS Volume Types

*   **General Purpose SSD (Solid State Drive) (gp2/gp3):** Balance of price and performance. Boot volumes, dev/test, low-latency apps.
*   **Provisioned IOPS (Input/Output Operations Per Second) SSD (Solid State Drive) (io1/io2):** Highest performance. Critical databases requiring sustained IOPS > 16,000.
*   **Throughput Optimized HDD (Hard Disk Drive) (st1):** Big data, data warehouses, log processing. Sequential I/O (Input/Output).
*   **Cold HDD (Hard Disk Drive) (sc1):** Lowest cost. Infrequent access file servers.

---

## 3.2 Design High-Performing and Elastic Compute Solutions

### 3.2.1 Compute Services

*   **Amazon EC2 (Elastic Compute Cloud):** Virtual servers. Total control.
    *   **Instance Families:**
        *   *General Purpose (T, M):* Web servers, code repositories.
        *   *Compute Optimized (C):* Batch processing, media transcoding, gaming servers.
        *   *Memory Optimized (R, X):* In-memory caches (Redis), real-time big data analytics.
        *   *Storage Optimized (I, D, H):* High I/O (Input/Output) data warehousing.
*   **AWS Lambda:** Serverless. Short running functions (<15 mins).
    *   *Performance:* RAM allocation determines CPU power. Increase RAM to increase CPU.
*   **Amazon ECS/EKS:** Containers. Efficient bin-packing of resources.

### 3.2.2 Scaling for Performance

*   **Placement Groups:** Control how instances are placed on hardware.
    *   **Cluster:** Pack instances close together (same rack/AZ). Low latency, high bandwidth (10Gbps+). *Risk: Rack failure.*
    *   **Partition:** Spread instances across logical partitions (different racks). Hadoop/Cassandra.
    *   **Spread:** Strictly separate hardware. Max 7 instances per AZ. Critical applications.

---

## 3.3 Determine High-Performing Database Solutions

### 3.3.1 Database Selection

| Requirement | Service | Engine |
| :--- | :--- | :--- |
| **Relational (SQL (Structured Query Language))** | **RDS (Relational Database Service) / Aurora** | MySQL, Postgres, Oracle, SQL Server. Use for structured data, joins, transactions (ACID (Atomicity, Consistency, Isolation, Durability)). |
| **NoSQL (Not Only SQL) (Key-Value)** | **DynamoDB** | High throughput, millisecond latency, simple queries. Shopping carts, user profiles. |
| **In-Memory** | **ElastiCache** | Microsecond latency. Caching, leaderboards, session store. |
| **Data Warehouse** | **Redshift** | OLAP (Online Analytical Processing). Complex queries on petabytes of data. Columnar storage. |

### 3.3.2 Caching Strategies

Caching relieves load on the backend database and improves latency for the user.

*   **Amazon ElastiCache (Redis/Memcached):** Sits between app and DB (Database). Application must be designed to check cache first (Lazy Loading or Write-Through).
*   **DynamoDB Accelerator (DAX):** Fully managed, highly available, in-memory cache for DynamoDB. *Requires no code changes.* Delivers microsecond latency.

### 3.3.3 Database Performance Features

*   **Read Replicas (RDS (Relational Database Service)/Aurora):** Asynchronous copies of the database. Route read-only queries (SELECT) to replicas to reduce load on the primary.
*   **RDS Proxy:** Pools and shares database connections to improve scalability, especially with Lambda.

---

## 3.4 Design High-Performing Network Architectures

*   **Amazon CloudFront:** Content Delivery Network (CDN). Caches static (and dynamic) content at Edge Locations closer to the user. Reduces latency and offloads origin.
*   **AWS Global Accelerator:** Ingress traffic enters the AWS (Amazon Web Services) network at the closest Edge Location and travels over the AWS backbone (not public internet). Consistent performance, reduced jitter.
*   **AWS Direct Connect:** Dedicated physical connection from on-premises to AWS. Consistent bandwidth and latency compared to VPN (Virtual Private Network) over internet.

---

## 3.5 Data Ingestion and Transformation

*   **Amazon Kinesis Data Streams:** Real-time ingestion of streaming data (logs, clickstreams) at scale. Manual consumption.
*   **Amazon Kinesis Data Firehose:** Ingests streaming data and automatically loads it into S3 (Simple Storage Service), Redshift, or OpenSearch. Near real-time (60s buffer).
*   **AWS Glue:** Serverless ETL (Extract, Transform, Load). Prepares data for analytics.
*   **AWS Snow Family:**
    *   *Snowball Edge:* Physical device to move TBs/PBs of data to AWS when network is slow.
    *   *Snowmobile:* Exabyte scale (shipping container).

---

### **Mini-Project 6: CloudMart Global Sale (Estimated Time: 1.5 Hours)**
*Goal: CloudMart is going global. Serve product images with low latency.*

1.  **Origin:** Launch `CloudMart-Image-Server` (EC2) in a region far from you (e.g., `ap-southeast-2`). Install Apache and put a large image `sale.jpg` in `/var/www/html`.
2.  **Direct Test:** Access `http://[EC2-Public-IP]/sale.jpg`. Measure the load time using your browser's Developer Tools (Network tab).
3.  **CDN Setup:** CloudMart needs a CDN. Create a **CloudFront Distribution** with the EC2 Public DNS as the Origin.
4.  **Accelerated Test:** Access the image via the CloudFront Domain Name.
    *   *First Hit:* Cache Miss (Slow).
    *   *Second Hit:* Cache Hit (Fast - served from Edge).
    *   Compare the times to quantify the benefit for CloudMart customers.
5.  **Geo-Restriction:** CloudMart can't sell to Embargoed countries. Configure CloudFront Geo-Restriction to **Block** your own country temporarily to verify it works.
6.  **Cleanup:** Disable and delete the CloudFront Distribution (wait for "Deployed" status). Terminate the EC2 instance.

---

# Domain 4: Design Cost-Optimized Architectures

Cost optimization focuses on avoiding unnecessary costs and selecting the most appropriate and right-sized resources.

## 4.1 Design Cost-Optimized Storage Solutions

### 4.1.1 S3 Storage Classes

*   **S3 (Simple Storage Service) Standard:** Frequent access. Expensive.
*   **S3 Intelligent-Tiering:** Automatically moves objects between frequent and infrequent access tiers based on usage. *No retrieval fees.* Great for unknown access patterns.
*   **S3 Standard-IA (Infrequent Access):** Cheaper storage, expensive retrieval. For data accessed less than once a month.
*   **S3 One Zone-IA (Infrequent Access):** Data stored in only 1 AZ (Availability Zone). cheaper (-20%). Risk of data loss if AZ (Availability Zone) fails.
*   **S3 Glacier:** Archival. Minutes to hours retrieval time. Very cheap.
*   **S3 Glacier Deep Archive:** Lowest cost. 12-48 hour retrieval.

*   **S3 (Simple Storage Service) Lifecycle Policies:** Automate moving data to colder tiers (e.g., Move to IA (Infrequent Access) after 30 days, Glacier after 90 days).

### 4.1.2 EBS Cost Optimization

*   **Delete unattached volumes:** EBS (Elastic Block Store) volumes persist (and cost money) even after the EC2 (Elastic Compute Cloud) instance is terminated, unless "Delete on Termination" is checked.
*   **Snapshots:** Delete old/stale snapshots.
*   **Volume Type:** Migrate `gp2` to `gp3` (often cheaper and allows independent scaling of IOPS (Input/Output Operations Per Second) and throughput).

## 4.2 Design Cost-Optimized Compute Solutions

### 4.2.1 EC2 Pricing Models

*   **On-Demand:** Pay by the second. No commitment. Spiky, short-term workloads.
*   **Reserved Instances (RI):** Commit to 1 or 3 years. Up to 72% discount. Steady-state usage (e.g., database).
    *   *Standard RI (Reserved Instance):* Can't change instance family. Higher discount.
    *   *Convertible RI (Reserved Instance):* Can change instance family. Lower discount.
*   **Savings Plans:** Commit to $X/hour usage for 1 or 3 years. More flexible than RIs (Reserved Instances). Applies to EC2 (Elastic Compute Cloud), Fargate, and Lambda.
*   **Spot Instances:** Bidding on unused capacity. Up to 90% discount. *Can be interrupted with 2 minutes notice.* Use for fault-tolerant, stateless apps, batch processing.

### 4.2.2 Right-Sizing

*   **AWS Compute Optimizer:** Uses Machine Learning (ML) to analyze historical metrics and recommend optimal instance types (e.g., "Downsize from m5.large to m5.medium").
*   Use **CloudWatch** metrics (CPU (Central Processing Unit), RAM (Random Access Memory)) to identify idle or underutilized resources.

## 4.3 Design Cost-Optimized Database and Network Architectures

### 4.3.1 Database Costs

*   **Serverless:** Aurora Serverless or DynamoDB On-Demand. Pay only when the DB is active/used. Good for infrequent/unpredictable workloads.
*   **Provisioned:** DynamoDB Provisioned Capacity. Cheaper if you have predictable steady load.

### 4.3.2 Network Costs

*   **Data Transfer IN:** Usually free.
*   **Data Transfer OUT:** Costs money.
*   **Private vs Public:**
    *   Data transfer between AZs (Availability Zones) in the same region costs money (if using public or elastic IPs (Internet Protocols)).
    *   Use **Private IPs** to communicate between instances in the same region to reduce cost.
*   **VPC (Virtual Private Cloud) Endpoints:** Using Gateway Endpoints for S3 (Simple Storage Service)/DynamoDB avoids NAT (Network Address Translation) Gateway processing charges.
*   **NAT (Network Address Translation) Gateway:** Charged per hour + per GB (Gigabyte) processed. Expensive for high throughput.

---

### **Mini-Project 7: Optimizing CloudMart's Spend (Estimated Time: 1 Hour)**
*Goal: Analyze the costs incurred during CloudMart's build and project future growth.*

1.  **AWS Calculator:** CloudMart is scaling up. Go to `calculator.aws` and estimate the monthly cost of the production architecture:
    *   2 Application Load Balancers.
    *   4 App Servers (t3.medium).
    *   1 Aurora Database (db.r5.large).
    *   1TB Data Transfer Out (CloudFront).
2.  **Cost Explorer:** Open Cost Explorer.
    *   Filter by Tag: `Project: CloudMart` (Ensure you tagged your resources in previous steps, or look at "Unblended Costs").
    *   Group by "Usage Type" to see what specifically cost money (e.g., `DataTransfer-Out-Bytes`).
3.  **Budgets:** Create a **Monthly Budget** for CloudMart capped at $10. Set up an alert to email you at 80% usage.
4.  **Tagging:** Go to the Billing Dashboard -> **Cost Allocation Tags**. Activate the `Project` tag. This ensures future CloudMart resources can be tracked separately from other projects.
5.  **Cleanup:** You can keep the budget active (it's free). Double-check all Regions to ensure all CloudMart resources (EC2, RDS, NAT Gateways, EIPs) are terminated.

---

## Conclusion

You have now covered the four major domains of the AWS Solutions Architect Associate exam.
1.  **Secure Architectures:** IAM (Identity and Access Management), VPC (Virtual Private Cloud), Encryption.
2.  **Resilient Architectures:** Auto Scaling, Multi-AZ (Availability Zone), Route 53.
3.  **High-Performing Architectures:** Caching, Global Accelerator, Right-sizing.
4.  **Cost-Optimized Architectures:** S3 (Simple Storage Service) Tiers, Spot Instances, Savings Plans.

**Next Steps:**
*   Review the hands-on assignments.
*   Take practice exams.
*   Review the AWS whitepapers for Well-Architected Framework.
*   Good luck!
