# AWS Solutions Architect Associate Learning Outcomes

## 1.1 Design secure access to AWS resources
- [ ] AWS security best practices (least privilege).
- [ ] AWS shared responsibility model.
- [ ] Federated access and identity services (IAM, IAM Identity Center/SSO).
- [ ] Designing flexible authorization using users, groups, roles, and policies.
- [ ] Securing IAM and root users (for example, enforcing MFA).
- [ ] Implementing role-based access control (STS, role switching, cross-account access).
- [ ] Choosing when to use resource-based policies.
- [ ] Managing access control across multiple AWS accounts.
- [ ] AWS global infrastructure (Regions, Availability Zones).
- [ ] Securing multi-account environments (Control Tower, SCPs).
- [ ] Determining when to federate external directory services with IAM roles.

## 1.2. Design secure workloads and applications
- [ ] Network traffic control (ports, protocols, flows).
- [ ] Secure application configuration and credential management.
- [ ] External threat vectors (DDoS, SQL injection).
- [ ] Secure application access patterns.
- [ ] Designing secure VPC architectures (security groups, NACLs, routing, NAT).
- [ ] Implementing network segmentation (public vs. private subnets).
- [ ] Integrating AWS security services (Shield, WAF, Secrets Manager, IAM Identity Center).
- [ ] AWS service endpoints.
- [ ] AWS security services and use cases (Cognito, GuardDuty, Macie).
- [ ] Securing external connectivity (VPN, Direct Connect).

## 1.3. Determine appropriate data security controls
- [ ] Encryption and key management principles.
- [ ] Data access control and governance.
- [ ] Data recovery strategies.
- [ ] Encrypting data at rest (KMS).
- [ ] Encrypting data in transit (TLS with ACM).
- [ ] Implementing backups and replication.
- [ ] Managing access policies for encryption keys.
- [ ] Data retention and classification.
- [ ] Rotating encryption keys and renewing certificates.
- [ ] Defining data access, lifecycle, and protection policies.
- [ ] Mapping AWS services to compliance requirements.

## 2.1. Design scalable and loosely coupled architectures
- [ ] Horizontal scaling and vertical scaling.
- [ ] Load balancing concepts (for example, ALB).
- [ ] Multi-tier architectures.
- [ ] Queuing and messaging concepts (publish/subscribe).
- [ ] Event-driven architectures.
- [ ] Design principles for microservices (stateless vs stateful).
- [ ] Serverless technologies and patterns (AWS Lambda, AWS Fargate).
- [ ] Storage types and characteristics (object, file, block).
- [ ] Caching strategies.
- [ ] Designing event-driven, microservice, and/or multi-tier architectures.
- [ ] Determining scaling strategies for architecture components.
- [ ] Determining AWS services required to achieve loose coupling.
- [ ] Determining when to use serverless technologies and patterns.
- [ ] Recommending appropriate compute, storage, networking, and database services.
- [ ] Using purpose-built AWS services for workloads.
- [ ] API creation and management (Amazon API Gateway, REST API).
- [ ] How to appropriately use edge accelerators (CDN).
- [ ] When to use read replicas.
- [ ] Workflow orchestration (AWS Step Functions).
- [ ] AWS managed services with use cases (Transfer Family, SQS, Secrets Manager).
- [ ] How to migrate applications into containers.
- [ ] The orchestration of containers (ECS, EKS).
- [ ] Note: SQS itself is must-know, but Transfer Family and deep container orchestration are not. They show up as distractors.
- [ ] Determining when to use containers.

## 2.2. Design highly available and/or fault-tolerant architectures
- [ ] AWS global infrastructure (Regions, AZs, Route 53).
- [ ] Disaster recovery strategies (backup/restore, pilot light, warm standby, active-active; RPO/RTO).
- [ ] Failover strategies.
- [ ] Load balancing concepts (ALB).
- [ ] Distributed design patterns.
- [ ] Storage options and durability characteristics.
- [ ] Immutable infrastructure.
- [ ] Basic networking concepts (route tables).
- [ ] Implementing designs to mitigate single points of failure.
- [ ] Selecting an appropriate DR strategy to meet business requirements.
- [ ] Determining AWS services for high availability across Regions or AZs.
- [ ] Implementing strategies to ensure data durability and availability (backups, replication).
- [ ] Using AWS services to improve reliability of legacy or non-cloud-native applications.
- [ ] Using purpose-built AWS services for workloads.
- [ ] Proxy concepts (Amazon RDS Proxy).
- [ ] Service quotas and throttling.
- [ ] Workload visibility (AWS X-Ray).
- [ ] AWS Managed Services (AMS) with use cases (Comprehend, Polly).
- [ ] Determining automation strategies to ensure infrastructure integrity.
- [ ] Identifying metrics based on business requirements.

## 3.1. Determine high-performing and/or scalable storage solutions
- [ ] Storage services with appropriate use cases (Amazon S3, EFS, EBS).
- [ ] Storage types and characteristics (object vs file vs block).
- [ ] Determining storage services and configurations that meet performance demands.
- [ ] Determining storage services that can scale to accommodate future needs.
- [ ] Hybrid storage solutions to meet business requirements.
- [ ] Hybrid storage appears rarely and only at a conceptual level.

## 3.2. Design high-performing and elastic compute solutions
- [ ] Scalability capabilities with appropriate use cases (EC2 Auto Scaling, AWS Auto Scaling).
- [ ] Serverless technologies and patterns (Lambda, Fargate).
- [ ] Queuing and messaging concepts (publish/subscribe).
- [ ] AWS compute services with appropriate use cases (Batch, EMR, Fargate).
- [ ] Distributed computing concepts supported by AWS infrastructure.
- [ ] Decoupling workloads so components scale independently.
- [ ] Identifying metrics and conditions to perform scaling actions.
- [ ] Selecting appropriate compute options and features (EC2 instance types).
- [ ] Selecting appropriate resource type and size (Lambda memory, etc.).
- [ ] The orchestration of containers (ECS, EKS).
- [ ] ECS/EKS are tested as “when to use”, not how to run them.

## 3.3. Determine high-performing database solutions
- [ ] Database types and services (relational vs non-relational, serverless, in-memory).
- [ ] Data access patterns (read-heavy vs write-heavy).
- [ ] Database replication (read replicas).
- [ ] Caching strategies and services (ElastiCache).
- [ ] Database capacity planning (IOPS, instance types, capacity units).
- [ ] Database engines with appropriate use cases.
- [ ] AWS global infrastructure (AZs, Regions).
- [ ] Determining an appropriate database type (Aurora vs DynamoDB, etc.).
- [ ] Designing database architectures.
- [ ] Integrating caching to meet business requirements.
- [ ] Configuring read replicas to meet business requirements.
- [ ] Determining an appropriate database engine (MySQL vs PostgreSQL).
- [ ] Database connections and proxies.

## 3.4. Determine high-performing and/or scalable network architectures
- [ ] Load balancing concepts (Application Load Balancer).
- [ ] Network connection options (VPN, Direct Connect, PrivateLink).
- [ ] How to design network architecture (subnet tiers, routing, IPs).
- [ ] Edge networking services with use cases (CloudFront, Global Accelerator).
- [ ] Creating network topologies (global, hybrid, multi-tier).
- [ ] Selecting the appropriate load balancing strategy.
- [ ] Determining appropriate placement of resources.
- [ ] Determining network configurations that scale.

## 3.5. Determine high-performing data ingestion and transformation solutions
- [ ] Streaming data services with appropriate use cases (Kinesis).
- [ ] Data ingestion patterns (frequency, batch vs stream).
- [ ] Data transformation services with use cases (AWS Glue).
- [ ] Data transfer services with use cases (DataSync, Storage Gateway).
- [ ] Sizes and speeds needed to meet business requirements.
- [ ] Designing data streaming architectures.
- [ ] Designing data transfer solutions.
- [ ] Building and securing data lakes.
- [ ] Selecting appropriate compute options for data processing (EMR).
- [ ] Selecting appropriate configurations for ingestion.
- [ ] Data analytics and visualization services (Athena, Lake Formation, QuickSight).
- [ ] Secure access to ingestion access points.
- [ ] Implementing visualization strategies.
- [ ] Transforming data between formats (.csv → .parquet).

## 4.1. Design cost-optimized storage solutions
- [ ] Storage types with associated characteristics (object, file, block).
- [ ] Storage tiering (for example, cold tiering for object storage).
- [ ] Storage access patterns.
- [ ] Backup strategies.
- [ ] Data lifecycles.
- [ ] Selecting the most cost-effective storage service for a workload.
- [ ] Managing S3 object lifecycles.
- [ ] Selecting the appropriate storage tier.
- [ ] Determining storage services and configurations for cost/performance.
- [ ] Selecting appropriate backup/archival solutions.
- [ ] AWS storage services with use cases (S3, EFS, EBS, FSx).
- [ ] Block storage options (HDD, SSD).
- [ ] Hybrid storage options (DataSync, Transfer Family, Storage Gateway).
- [ ] Access options (Requester Pays, etc.).
- [ ] AWS cost management tools/features.
- [ ] Determining storage size for a workload.
- [ ] Determining when storage auto scaling is required.
- [ ] Selecting service for data migration.
- [ ] Selecting data lifecycle for storage.
- [ ] Designing storage strategies (batch vs individual uploads).

## 4.2. Design cost-optimized compute solutions
- [ ] AWS purchasing options (Spot, Reserved Instances, Savings Plans).
- [ ] Scaling strategies (Auto Scaling, hibernation).
- [ ] Optimization of compute utilization (containers, serverless, microservices).
- [ ] Instance types, families, and sizes.
- [ ] AWS global infrastructure (AZs, Regions).
- [ ] Determining cost-effective AWS compute services (Lambda, EC2, Fargate).
- [ ] Determining appropriate scaling methods for elastic workloads.
- [ ] Selecting appropriate instance family for a workload.
- [ ] Selecting appropriate instance size for a workload.
- [ ] Determining required availability for workload classes (prod vs non-prod).
- [ ] Distributed compute strategies (edge processing).
- [ ] Hybrid compute options (Outposts, Snowball Edge).
- [ ] AWS cost management tools/features.
- [ ] Determining load balancing strategy (ALB vs NLB vs GWLB).

## 4.3. Design cost-optimized database solutions
- [ ] Database types and services (relational vs non-relational, Aurora, DynamoDB).
- [ ] Database replication (read replicas).
- [ ] Database engines with appropriate use cases (MySQL, PostgreSQL, migrations).
- [ ] Database capacity planning (IOPS, instance types, capacity units).
- [ ] Data retention policies.
- [ ] Caching strategies.
- [ ] Determining cost-effective AWS database services/types (RDS, DynamoDB, serverless).
- [ ] Determining appropriate database engine (MySQL vs PostgreSQL).
- [ ] Designing appropriate backup and retention policies (snapshots, frequency).
- [ ] Integrating caching to meet business requirements.
- [ ] AWS cost management tools/features.
- [ ] Database connections and proxies.
- [ ] Migrating database schemas/data across locations or engines.

## 4.3. Design cost-optimized network architectures
- [ ] Load balancing concepts (ALB).
- [ ] NAT gateway costs and comparison (shared vs per-AZ).
- [ ] Network connectivity options (VPN, Direct Connect, private lines).
- [ ] Network routing, topology, and peering (Transit Gateway, VPC peering).
- [ ] Network services with use cases (DNS).
- [ ] Configuring network routes to minimize costs (Region→Region, AZ→AZ, VPC endpoints, Global Accelerator).
- [ ] Configuring appropriate NAT gateway types (shared vs per-AZ).
- [ ] Configuring appropriate network connections (Direct Connect vs VPN vs Internet).
- [ ] Selecting appropriate throttling strategy.
- [ ] Determining strategic needs for CDNs and edge caching.
- [ ] AWS cost management tools/features.
- [ ] Reviewing existing workloads for network optimizations.
- [ ] Selecting appropriate bandwidth allocation for network devices.
