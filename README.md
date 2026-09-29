# 🗺️ AWS CLF-C02 Ultimate Master Study Guide: The Neighborhood Approach

This is your comprehensive, deep-dive study guide for the AWS Certified Cloud Practitioner (CLF-C02) exam. It uses the "Modern City" analogy to categorize services, mapping out the specific limits, pricing models, architectural rules, and frameworks you need to ace the exam.

---

## 🧭 Navigator & Quick Reference

### 🏢 Cloud Service Models (IaaS vs. PaaS vs. SaaS)
*Before diving into the neighborhoods, understand how AWS services map to cloud models. As you move down this list, AWS takes on more management, and you take on less.*

| Model | Definition | AWS Examples | Your Responsibility |
| :--- | :--- | :--- | :--- |
| **IaaS** (Infrastructure) | Rent raw compute/storage. Max control, max management. | EC2, EBS, EFS, VPC, S3 | OS patching, app config, data security, IAM. |
| **PaaS** (Platform) | Rent a managed environment. Bring your code/data. | RDS, Lambda, Elastic Beanstalk, DynamoDB | App code, data security, IAM. |
| **SaaS** (Software) | Rent a finished application. Just use it. | Chime, WorkDocs, Connect, Amazon Managed Services | Data security, user access (IAM). |

### 🗂️ Table of Contents (The 9 Neighborhoods)
1. [Neighborhood 1: Compute (The Buildings & Workforce)](#-neighborhood-1-compute-the-buildings--workforce)
2. [Neighborhood 2: Storage (The Warehouses)](#-neighborhood-2-storage-the-warehouses)
3. [Neighborhood 3: Databases (The Filing Cabinets)](#-neighborhood-3-databases-the-filing-cabinets)
4. [Neighborhood 4: Networking & Content Delivery (The Roads)](#-neighborhood-4-networking--content-delivery-the-roads)
5. [Neighborhood 5: Security, Identity, & Compliance (The Police)](#-neighborhood-5-security-identity--compliance-the-police)
6. [Neighborhood 6: Management, Monitoring, & Billing (City Hall)](#-neighborhood-6-management-monitoring--billing-city-hall)
7. [Neighborhood 7: Support, Migration & Cloud Concepts (The City Planners)](#-neighborhood-7-support-migration--cloud-concepts-the-city-planners)
8. [Neighborhood 8: Well-Architected Framework (The Building Code)](#-neighborhood-8-well-architected-framework-the-building-code)
9. [Neighborhood 9: Cloud Adoption Framework (The Master City Plan)](#-neighborhood-9-cloud-adoption-framework-the-master-city-plan)
10. [The Shared Responsibility Model (The Golden Rule)](#️-the-shared-responsibility-model-the-golden-rule)
11. [The Ultimate "Magic Words" Cheat Sheet](#-the-ultimate-magic-words-cheat-sheet)
12. [Exam Day Strategy](#-exam-day-strategy)

---

## 🏗️ Neighborhood 1: Compute (The Buildings & Workforce)
*Where the actual processing happens. The exam tests your ability to choose the right compute based on control vs. management overhead.*

### 1. Amazon EC2 (Elastic Compute Cloud) - *IaaS*
*   **Analogy:** Renting a plot of land and building a house yourself. You handle the maintenance, but you have total control.
*   **Instance Families (Know the use cases):**
    *   **General Purpose (M, T):** Balanced compute, memory, networking (e.g., web servers, dev environments). *T-series provides burstable performance.*
    *   **Compute Optimized (C):** High-performance processors (e.g., batch processing, gaming servers).
    *   **Memory Optimized (R, X):** Fast performance for large datasets in memory (e.g., real-time big data analytics, in-memory databases).
    *   **Storage Optimized (I, D):** High, sequential read/write access to large datasets on local storage (e.g., NoSQL databases, data warehousing).
    *   **Accelerated Computing (P, G, F):** Hardware accelerators/GPUs (e.g., machine learning, video transcoding).
*   **Pricing Models (⭐ CRITICAL):**
    *   **On-Demand:** Pay by the second/hour. No commitment. Best for short-term, spiky, or unpredictable workloads.
    *   **Savings Plans / Reserved Instances (RI):** 1- or 3-year commitment for up to 72% discount. Best for steady-state, predictable workloads.
    *   **Spot Instances:** Bid on unused AWS capacity for up to 90% off. **Catch:** AWS can terminate it with a 2-minute warning. Best for fault-tolerant, stateless, batch processing.
    *   **Dedicated Hosts:** A physical server dedicated entirely to you. Used *only* for strict regulatory compliance or specific software licensing (e.g., "Bring Your Own License" tied to physical CPU sockets).
    *   **Dedicated Instances:** Run on hardware dedicated to your account, but you have no visibility into the physical server (not sufficient for socket/core licensing).

### 2. AWS Lambda (Serverless Compute) - *PaaS*
*   **Analogy:** Hiring a freelancer for a single, specific task. You don't provide a desk; you just pay for the exact minutes they work.
*   **Exam Essentials:** 
    *   **Event-driven:** Triggered by S3 uploads, DynamoDB streams, or API Gateway.
    *   **Hard Limit:** Maximum execution time is **15 minutes**. If a task takes longer, you *must* use ECS, Fargate, or Batch.
    *   **Pricing:** Pay per request (first 1 million/month are free) + duration (measured in milliseconds, based on allocated memory).

### 3. AWS Elastic Beanstalk (PaaS)
*   **Analogy:** Renting a fully furnished, move-in-ready apartment. You bring your belongings (code); the landlord handles plumbing and maintenance.
*   **Exam Essentials:** You upload your code (Java, .NET, Node.js, Python, etc.), and AWS automatically provisions **EC2, Auto Scaling Groups, Elastic Load Balancers, and CloudWatch**. You pay *only* for the underlying resources, not for Beanstalk itself.

### 4. Containers (ECS / EKS / Fargate)
*   **Amazon ECS:** AWS’s native container orchestration service.
*   **Amazon EKS:** Managed Kubernetes service (use if you already have Kubernetes expertise).
*   **AWS Fargate:** The **serverless** compute engine for containers. You do not provision or manage *any* EC2 instances. 

---

##  Neighborhood 2: Storage (The Warehouses)
*The exam tests if you know the difference between Block, File, and Object storage, and how to optimize costs.*

### 1. Amazon S3 (Simple Storage Service) - *Object Storage (IaaS)*
*   **Analogy:** A massive, infinitely scalable digital warehouse. You drop off boxes (Objects) and get a unique claim ticket (URL).
*   **Exam Essentials:**
    *   **Durability:** **99.999999999% (11 nines)**. This is a guaranteed exam fact.
    *   **Bucket Naming Rules:** Globally unique, 3-63 characters, lowercase letters, numbers, hyphens, and periods only. No underscores.
    *   **Storage Classes:**
        *   **S3 Standard:** Frequent access, low latency.
        *   **S3 Intelligent-Tiering:** Automatically moves data between tiers based on access patterns. No retrieval fees. Best for *unknown* access patterns.
        *   **S3 Standard-IA:** Infrequent access, but requires *milliseconds* retrieval. Has a retrieval fee.
        *   **S3 Glacier Flexible Retrieval:** Archival. Retrieval takes minutes to hours.
        *   **S3 Glacier Deep Archive:** Lowest cost. Retrieval takes 12 to 48 hours.
    *   **Key Features:** **Versioning** (protects against accidental deletes/overwrites), **Lifecycle Policies** (automate moving data to cheaper tiers), **Cross-Region Replication (CRR)** (async copy to another region, requires Versioning), **Object Lock** (WORM model: Write Once, Read Many. *Compliance Mode* means not even the Root user can delete it before the retention period ends).

### 2. Amazon EBS (Elastic Block Store) - *Block Storage (IaaS)*
*   **Analogy:** The internal hard drive of your personal computer.
*   **Exam Essentials:** 
    *   Attaches to a **single EC2 instance** and is locked to a **single Availability Zone (AZ)**.
    *   **Pricing:** You pay for **provisioned** capacity (e.g., if you create a 100GB volume, you pay for 100GB, even if it's empty).
    *   **Snapshots:** Point-in-time backups of EBS volumes. They are incremental and stored in **S3**.

### 3. Amazon EFS (Elastic File System) - *File Storage (IaaS)*
*   **Analogy:** A shared network drive (like a mapped `Z:` drive) in a corporate office.
*   **Exam Essentials:** 
    *   Uses the NFS protocol (Linux only). 
    *   Can be mounted by **hundreds of EC2 instances simultaneously** across **multiple AZs**.
    *   **Pricing:** Pay only for the storage **you actually consume** (it grows and shrinks automatically).

### 4. AWS Storage Gateway (Hybrid Storage)
*   **File Gateway:** Presents S3 as an NFS/SMB file share to on-premises servers.
*   **Volume Gateway:** Presents S3 as iSCSI block storage to on-premises servers.
*   **Tape Gateway:** Replaces physical backup tapes with virtual tapes stored in S3/Glacier.

---

## 🗄️ Neighborhood 3: Databases (The Filing Cabinets)
*Structured and unstructured data storage. Know the difference between OLTP (transactions) and OLAP (analytics).*

### 1. Amazon RDS (Relational Database Service) - *PaaS*
*   **Analogy:** A highly organized, traditional filing cabinet with strict rows and columns.
*   **Exam Essentials:** Managed service (AWS handles OS patching, backups, hardware). Supports MySQL, PostgreSQL, MariaDB, Oracle, SQL Server.
*   **Amazon Aurora:** AWS’s flagship relational DB. Compatible with MySQL/PostgreSQL, but **5x faster than MySQL** and **3x faster than PostgreSQL**. Automatically scales storage in 10GB increments up to 128TB.
*   **High Availability vs. Performance:**
    *   **Multi-AZ Deployment:** Synchronous standby replica in a different AZ. Used for **Disaster Recovery / High Availability**. *Does not improve read performance.*
    *   **Read Replicas:** Asynchronous copies. Used to scale **read performance**. You can have up to 5 Read Replicas per primary instance.

### 2. Amazon DynamoDB (NoSQL) - *PaaS*
*   **Analogy:** A massive, flexible box of index cards. Instantly find any card by its unique label (Key).
*   **Exam Essentials:** Fully serverless, key-value and document database. **Single-digit millisecond latency** at any scale. Best for mobile apps, gaming leaderboards, and shopping carts. No complex "JOIN" operations.

### 3. Amazon Redshift (Data Warehouse) - *PaaS*
*   **Analogy:** A specialized research library designed to analyze millions of documents at once.
*   **Exam Essentials:** Columnar storage, Massively Parallel Processing (MPP). Used for **OLAP** (complex analytics, business intelligence, petabytes of historical data). *Not* for high-frequency transactional updates.

### 4. Amazon ElastiCache (In-Memory Caching) - *PaaS*
*   **Analogy:** Keeping your most frequently used documents on your desk instead of walking to the filing cabinet.
*   **Exam Essentials:** Supports **Redis** (multi-AZ, advanced data types, persistence) and **Memcached** (simple, multi-node, no persistence). Used to reduce database load and improve read latency.

---

## ️ Neighborhood 4: Networking & Content Delivery (The Roads)
*How data moves securely and quickly. This is where the exam tests your understanding of boundaries and traffic flow.*

### 1. Amazon VPC (Virtual Private Cloud)
*   **Analogy:** A gated community with its own private roads.
*   **Exam Essentials:**
    *   **Subnets:** Divide a VPC. **Public Subnets** have a Route Table pointing to an **Internet Gateway (IGW)**. **Private Subnets** do not.
    *   **NAT Gateway:** Allows instances in a *Private Subnet* to initiate outbound traffic to the internet (e.g., for software updates) while preventing inbound traffic from the internet. It is deployed in a *Public Subnet*. It is **AZ-specific** (for high availability, deploy one in each AZ).
    *   **VPC Endpoints:** Allow private connectivity to AWS services *without* traversing the public internet.
        *   **Gateway Endpoint:** Free. Used *only* for **S3** and **DynamoDB**.
        *   **Interface Endpoint (AWS PrivateLink):** Costs money. Uses Elastic Network Interfaces (ENIs) for other services (e.g., SQS, Kinesis).

### 2. Security Groups vs. Network ACLs (NACLs) ⭐ CRITICAL
| Feature | Security Group (SG) | Network ACL (NACL) |
| :--- | :--- | :--- |
| **Level of Operation** | **Instance** level (The house) | **Subnet** level (The neighborhood gate) |
| **Statefulness** | **Stateful** (Return traffic is automatically allowed) | **Stateless** (Must explicitly allow return traffic) |
| **Rule Types** | **Allow** rules only (Default: Deny all) | **Allow** and **Deny** rules (Default: Allow all) |
| **Evaluation Order** | All rules are evaluated before deciding | Evaluated in **numerical order** (lowest number first) |

### 3. Amazon CloudFront (CDN)
*   **Analogy:** Local distribution centers so customers get packages faster without going to the main warehouse.
*   **Exam Essentials:** Caches static and dynamic content at global **Edge Locations**. Reduces latency and offloads traffic from the origin. 
*   *Billing Fact:* Data transfer from an origin (like S3) to CloudFront is **FREE**.

### 4. Amazon Route 53 (DNS)
*   **Analogy:** The internet’s phonebook and GPS.
*   **Exam Essentials:** 
    *   **Record Types:** **A** (IPv4), **AAAA** (IPv6), **CNAME** (maps to another domain name, *cannot* be used for root/naked domains), **Alias** (AWS-specific, maps to AWS resources like ELB/S3, *can* be used for root domains, and is **free**).
    *   **Routing Policies:** **Simple**, **Weighted** (split traffic by %), **Latency** (route to region with lowest network latency), **Failover** (active-passive, requires Health Checks), **Geolocation** (route based on user's physical country/continent).

---

## ️ Neighborhood 5: Security, Identity, & Compliance (The Police)
*Makes up ~30% of the exam. Focus on the Shared Responsibility Model and least privilege.*

### 1. AWS IAM (Identity and Access Management)
*   **Analogy:** Employee ID badges and keycards.
*   **Exam Essentials:** 
    *   **Global** service (not tied to a Region).
    *   **Users:** Actual people or apps (long-term credentials).
    *   **Groups:** Collections of users (apply permissions to the group, not individual users).
    *   **Roles:** Temporary credentials for **AWS Services** (e.g., EC2 accessing S3) or external federated users. *Roles do not have passwords or long-term access keys.*
    *   **Policies:** JSON documents. **Explicit Deny always overrides any Allow.**

### 2. Encryption & Key Management
*   **AWS KMS:** Managed service to create and control encryption keys. Multi-tenant (AWS manages the underlying hardware).
*   **AWS CloudHSM:** Single-tenant, dedicated Hardware Security Modules (HSMs). Required for strict compliance like **FIPS 140-2 Level 3**.

### 3. Threat Protection
*   **AWS WAF:** Web Application Firewall (Layer 7). Protects against SQL injection and Cross-Site Scripting (XSS).
*   **AWS Shield:** DDoS protection (Layer 3/4). **Shield Standard** is free and automatic. **Shield Advanced** is paid, provides 24/7 access to the DDoS Response Team (DRT), and offers cost protection against scaling charges during an attack.
*   **Amazon GuardDuty:** Intelligent threat detection using ML (analyzes VPC Flow Logs, CloudTrail, and DNS logs to find compromised instances or crypto-mining).
*   **Amazon Macie:** Uses ML to discover, classify, and protect sensitive data (PII, PHI, credit cards) specifically in **Amazon S3**.

### 4. Compliance & Auditing
*   **AWS Artifact:** On-demand portal to download AWS compliance reports (SOC 1/2/3, PCI DSS, ISO certifications) for your auditors.

---

## 🏛️ Neighborhood 6: Management, Monitoring, & Billing (City Hall)
*How you keep track of costs, performance, and best practices.*

### 1. Monitoring & Auditing
*   **Amazon CloudWatch:** Monitors **performance metrics** (CPU, memory, disk I/O) and collects logs. Can trigger **Alarms** (e.g., "Email me if CPU > 80%").
*   **AWS CloudTrail:** Logs **API calls** and user activity. Answers: *"Who did what, when, and from which IP address?"* Retains 90 days of event history for free.
*   **AWS Config:** Tracks **resource configuration changes** over time and evaluates compliance against rules (e.g., "Alert me if any S3 bucket becomes public").

### 2. Billing & Cost Management
*   **AWS Budgets:** Set custom cost or usage budgets and receive **alerts** (email/SNS) when you are forecasted to exceed them. (Up to 20 free budgets).
*   **AWS Cost Explorer:** Visual tool with graphs to analyze historical spending and forecast future costs.
*   **AWS Cost & Usage Report:** The most granular billing data (CSV format), delivered to an S3 bucket.
*   **Consolidated Billing (via AWS Organizations):** Link multiple AWS accounts to get a single bill, combine usage for **volume pricing discounts**, and share Reserved Instance/Savings Plan benefits across the organization.

### 3. AWS Trusted Advisor
*   **Analogy:** An automated city inspector.
*   **Exam Essentials:** Checks your account across 5 pillars: Cost Optimization, Security, Fault Tolerance, Performance, and Service Limits. 
*   *Trap:* Only **7 core checks** (mostly security and service limits) are free for *all* users. Full Cost Optimization checks require a **Business or Enterprise Support Plan**.

### 4. AWS Systems Manager (SSM)
*   **Exam Essentials:** A unified interface for managing AWS resources. 
    *   **Patch Manager:** Automates OS patching.
    *   **Session Manager:** Provides secure, auditable shell access to EC2 instances **without opening inbound port 22 (SSH)**.

---

## 🤝 Neighborhood 7: Support, Migration & Cloud Concepts (The City Planners)
*Critical Cloud Concepts, Billing, and Migration topics that complete your CLF-C02 knowledge.*

### 1. AWS Support Plans
*   **Basic (Free):** 24/7 customer service, AWS Personal Health Dashboard, **7 core Trusted Advisor checks**.
*   **Developer:** Business-hours email access to Cloud Support Associates.
*   **Business:** **24/7 phone, email, and chat support**. **< 1-hour response time** for "Production System Impaired". Unlocks **Full Trusted Advisor checks**.
*   **Enterprise:** **< 15-minute response time** for "Business-Critical System Down". Includes a dedicated **Technical Account Manager (TAM)** and Infrastructure Event Management.

### 2. The 6 Advantages of Cloud Computing
1. Trade capital expense (CapEx) for variable expense (OpEx).
2. Benefit from massive economies of scale.
3. Stop guessing capacity.
4. Increase speed and agility.
5. Stop spending money running and maintaining data centers.
6. Go global in minutes.

### 3. The 6 Rs of Cloud Migration
1. **Rehost ("Lift and Shift"):** Move to EC2 without changes. Fastest.
2. **Replatform ("Lift, Tinker, and Shift"):** Move to RDS. Minor optimizations.
3. **Repurchase ("Drop and Shop"):** Move to SaaS (e.g., Salesforce).
4. **Refactor / Re-architect:** Rewrite for cloud-native (e.g., Lambda). Most expensive, highest ROI.
5. **Retire:** Turn off unneeded apps.
6. **Retain:** Keep on-premises (compliance/legacy).

### 4. Advanced Billing Tools
*   **AWS Free Tier:** 12 Months Free, Always Free, and Trials.
*   **Cost Allocation Tags:** Metadata to track costs. *Must be activated in the Billing console to work.*
*   **Consolidated Billing:** Single bill + Volume Pricing Discounts + Shared RI benefits.

---

## 📐 Neighborhood 8: Well-Architected Framework (The Building Code)
*Best practices to help you build secure, high-performing, resilient, and efficient infrastructure.*

### The 6 Pillars (⭐ CRITICAL EXAM KNOWLEDGE):
1. **Operational Excellence:** Run and monitor systems, automate deployments, learn from failures.
2. **Security:** Protect information/assets. Least Privilege, encryption, incident response.
3. **Reliability:** Recover from disruptions. Multi-AZ, Auto Scaling, decoupled architectures.
4. **Performance Efficiency:** Use resources efficiently. Right-sizing, serverless, caching.
5. **Cost Optimization:** Avoid unnecessary costs. Managed services, Spot Instances, lifecycle policies.
6. **Sustainability:** Minimize environmental impacts. Maximize utilization, select renewable Regions.

*Tool:* **AWS Well-Architected Tool** (Free tool in the console to review workloads against these pillars).

---

## 🗺️ Neighborhood 9: Cloud Adoption Framework (The Master City Plan)
*Guidance to help organizations develop an efficient plan for their cloud adoption journey (Organizational Change).*

### The 6 Perspectives:
#### 👔 Business & People (The "Soft" Side):
1. **Business:** Align IT to business needs. Measure ROI, business case development.
2. **People:** Address staffing, training, and cultural changes. "Cloud champions".
3. **Governance:** Maximize benefits, minimize risks. Establish policies, compliance, risk management.

#### 💻 Technology (The "Hard" Side):
4. **Platform:** IT infrastructure and cloud architecture. VPC design, service selection.
5. **Security:** Confidentiality, integrity, availability. IAM, detective controls, encryption.
6. **Operations:** Day-to-day management, monitoring (CloudWatch), incident management, help desk.

---

## ⚖️ The Shared Responsibility Model (The Golden Rule)

| Responsibility Area | AWS Responsibility (Security **OF** the Cloud) | Customer Responsibility (Security **IN** the Cloud) |
| :--- | :--- | :--- |
| **Physical Infrastructure** | Data centers, physical servers, storage drives, physical security. | *None* |
| **Network Infrastructure** | Global network backbone, Regions, AZs, Edge Locations. | Configuring VPCs, Subnets, Route Tables, Security Groups. |
| **Compute & Hypervisor** | The underlying host OS, hypervisor, and virtualization layer. | **IaaS (EC2):** Guest OS patching, app installation.<br>**PaaS/SaaS:** *AWS handles this.* |
| **Data & Content** | Ensuring the *durability* of the storage infrastructure. | Encrypting data, classifying data, managing backups. |
| **Identity & Access** | Providing the IAM service infrastructure. | Creating IAM Users/Roles, managing passwords, enforcing Least Privilege. |

*Rule of Thumb:* As you move from IaaS (EC2) to PaaS (RDS/Lambda) to SaaS (Chime), AWS takes on more management responsibility, but **you are ALWAYS responsible for your data and your IAM access.**

---

## 🎯 The Ultimate "Magic Words" Cheat Sheet

When you see these phrases in an exam question, the answer is almost always the corresponding service or concept:

**Compute & Storage:**
*   **"Decouple" or "Buffer"** → Amazon SQS
*   **"Broadcast" or "Fan-out"** → Amazon SNS
*   **"15-minute limit"** → AWS Lambda (If longer, use Fargate/Batch)
*   **"Physical socket/core licensing"** → Dedicated Hosts
*   **"Immutable" + "Root user cannot delete"** → S3 Object Lock (Compliance Mode)
*   **"Interruptible" + "Fault-tolerant" + "Lowest Cost"** → Spot Instances

**Databases & Networking:**
*   **"Single-digit millisecond latency" + "Key-value"** → Amazon DynamoDB
*   **"Complex queries" + "Relationships" + "SQL"** → Amazon RDS
*   **"Data warehouse" + "Analytics" + "Petabytes"** → Amazon Redshift
*   **"Speed up" + "Frequent reads" + "In-memory"** → Amazon ElastiCache
*   **"Global users" + "Low latency" + "Cache static content"** → Amazon CloudFront
*   **"Translate domain names" / "Route traffic"** → Amazon Route 53
*   **"Explicit Deny" rule** → Network ACL (NACL)
*   **"Transitive routing between many VPCs"** → AWS Transit Gateway

**Security & Management:**
*   **"Temporary credentials for an EC2 instance"** → IAM Role
*   **"Who did what?" + "API calls"** → AWS CloudTrail
*   **"Resource configuration over time" + "Compliance"** → AWS Config
*   **"Performance metrics" + "Alarms"** → Amazon CloudWatch
*   **"Cost alerts" + "Budget thresholds"** → AWS Budgets
*   **"Download compliance reports (SOC, PCI)"** → AWS Artifact
*   **"Secure shell access without opening port 22"** → AWS Systems Manager (Session Manager)

**Frameworks & Support:**
*   **"Technical Account Manager (TAM)"** → Enterprise Support Plan
*   **"24/7 phone support" + "Production system impaired"** → Business Support Plan
*   **"Review a workload against best practices"** → AWS Well-Architected Tool
*   **"Recover from failure" / "Survive an AZ outage"** → Well-Architected **Reliability** Pillar
*   **"Automate deployments" / "Learn from failures"** → Well-Architected **Operational Excellence** Pillar
*   **"Minimize environmental impact" / "Reduce carbon footprint"** → Well-Architected **Sustainability** Pillar
*   **"Measure ROI" / "Align IT to business outcomes"** → CAF **Business** Perspective
*   **"Staff training" / "Cloud champions" / "Culture change"** → CAF **People** Perspective
*   **"Policies, compliance, risk management"** → CAF **Governance** Perspective
*   **"Day-to-day management, monitoring, help desk"** → CAF **Operations** Perspective
*   **"Lift and shift"** → Rehost
*   **"Lift, tinker, and shift" (e.g., moving to RDS)** → Replatform
*   **"Drop and shop" (e.g., moving to Salesforce)** → Repurchase
*   **"Most granular billing data" + "CSV to S3"** → AWS Cost and Usage Report
*   **"Combine usage for volume discounts"** → Consolidated Billing (AWS Organizations)
*   **"Track costs by department or project"** → Cost Allocation Tags

---

## 🧠 Exam Day Strategy

1. **Identify the Constraint:** Every question has a hidden constraint. Is it asking for the *"most cost-effective"* solution? The *"least operational overhead"*? The *"highest performance"*? The constraint will eliminate half the wrong answers.
2. **Beware of Absolute Words:** If an answer choice says "always" or "never," it is usually wrong. AWS architectures are about trade-offs.
3. **Eliminate AWS Anti-Patterns:** Immediately cross out answers that suggest hardcoding credentials, making S3 buckets public for backend processing, or using EC2 for a simple 3-second script.
4. **Manage Your Time:** You have 90 minutes for 65 questions (~1.3 minutes per question). If you are stuck, flag it and move on.
