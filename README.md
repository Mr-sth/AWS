# ☁️ AWS Certified Cloud Practitioner (CLF-C02)
### 📚 Module-Wise Study Syllabus

![Exam](https://img.shields.io/badge/Exam-CLF--C02-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Level](https://img.shields.io/badge/Level-Foundational-232F3E?style=for-the-badge)
![Questions](https://img.shields.io/badge/Questions-~65-3B82F6?style=for-the-badge)
![Time](https://img.shields.io/badge/Time-90_min-10B981?style=for-the-badge)
![Pass](https://img.shields.io/badge/Pass-700%2F1000-A855F7?style=for-the-badge)

> 💡 **How to use this file:** work through the modules in order, tick the boxes as you finish, and do the lab and checkpoint for every module. Module titles other than 2 and 10 are a standard layout; rename to match your course.

---

## 🎯 Exam Domains at a Glance

| Domain | Weight | Share of exam |
|:--|:--:|:--|
| 🔵 **1. Cloud Concepts** | 24% | `████████░░░░░░░░░░░░` |
| 🔴 **2. Security and Compliance** | 30% | `██████████░░░░░░░░░░` |
| 🟢 **3. Cloud Technology and Services** | 34% | `███████████░░░░░░░░░` |
| 🟣 **4. Billing, Pricing and Support** | 12% | `████░░░░░░░░░░░░░░░░` |

> 🔥 Domains 2 and 3 together are about **two-thirds** of the exam. Spend the most time there.

## 🗺️ Module Map

| ✅ | # | Module | Domain | ⏱️ Time |
|:--:|:--:|:--|:--:|:--:|
| ⬜ | 1 | ☁️ Introduction to Cloud and AWS | 🔵 1 | 4–5 h |
| ⬜ | 2 | 🖥️ Compute in the Cloud | 🟢 3 | 6–8 h |
| ⬜ | 3 | 🌍 Global Infrastructure and Reliability | 🔵 1 / 🟢 3 | 3–4 h |
| ⬜ | 4 | 🌐 Networking | 🟢 3 / 🔴 2 | 5–6 h |
| ⬜ | 5 | 🗄️ Storage | 🟢 3 | 5–6 h |
| ⬜ | 6 | 🛢️ Databases | 🟢 3 | 4–5 h |
| ⬜ | 7 | 🔐 Security and Identity | 🔴 2 | 7–9 h |
| ⬜ | 8 | 💰 Pricing, Billing and Support | 🟣 4 | 4–5 h |
| ⬜ | 9 | 🚀 Migration, Adoption and Innovation | 🔵 1 / 🟢 3 | 5–6 h |
| ⬜ | 10 | 📊 Monitoring, Compliance and Governance | 🔴 2 / 🟢 3 | 5–6 h |

---

## ☁️ Module 1 · Introduction to Cloud and AWS
`🔵 Domain 1` `⏱️ 4–5 h`

### 📖 Topics
- Cloud models: **IaaS, PaaS, SaaS**; deployment: cloud, hybrid, on-premises
- Six benefits of cloud: trade CapEx for OpEx, economies of scale, stop guessing capacity, speed and agility, no data-center spend, go global in minutes
- Elasticity vs scalability vs high availability vs fault tolerance
- **Well-Architected pillars:** Operational Excellence · Security · Reliability · Performance Efficiency · Cost Optimization · Sustainability
- Access: Console, CLI, SDKs, CloudShell

> 💡 **Exam tip:** match each scenario to a pillar (e.g., "recover from failure" → Reliability).

> 🧪 **Lab:** create an AWS account, enable root MFA, set a budget alert.

> ✅ **Checkpoint:** explain elasticity vs scalability in two sentences.

- [ ] Module 1 complete

---

## 🖥️ Module 2 · Compute in the Cloud
`🟢 Domain 3` `⏱️ 6–8 h`

### 📖 Topics
- **EC2:** instance families, AMIs, user data
- **Purchasing options**

| Option | Best for |
|:--|:--|
| On-Demand | Short, unpredictable workloads |
| Reserved / Savings Plans | Steady, long-term usage |
| Spot | Interruptible, cheapest |
| Dedicated Hosts | Licensing and compliance |

- **Auto Scaling** and **Elastic Load Balancing** (ALB, NLB, GWLB)
- **Containers:** ECS, EKS, Fargate, ECR
- **Serverless:** Lambda; **decoupling:** SQS, SNS, EventBridge, Step Functions

> 💡 **Exam tip:** "no servers to manage" → Lambda or Fargate.

> 🧪 **Lab:** launch and terminate an EC2 instance; trigger a Lambda function.

> ✅ **Checkpoint:** which purchasing option for an interruptible batch job?

- [ ] Module 2 complete

---

## 🌍 Module 3 · Global Infrastructure and Reliability
`🔵 Domain 1` `🟢 Domain 3` `⏱️ 3–4 h`

### 📖 Topics
- **Regions, Availability Zones, edge locations**
- Region choice: 📜 compliance · ⚡ latency · 🧩 service availability · 💲 pricing
- **CloudFront**, **Route 53**, **Global Accelerator**
- Local Zones, Wavelength, Outposts

> 💡 **Exam tip:** data never leaves a Region unless you move it.

> 🧪 **Lab:** serve an S3 static site through CloudFront.

> ✅ **Checkpoint:** name the four Region selection factors.

- [ ] Module 3 complete

---

## 🌐 Module 4 · Networking
`🟢 Domain 3` `🔴 Domain 2` `⏱️ 5–6 h`

### 📖 Topics
- **VPC:** subnets, route tables, Internet Gateway, NAT Gateway
- **Security Group vs NACL**

| | Security Group | NACL |
|:--|:--|:--|
| Level | Instance | Subnet |
| State | Stateful | Stateless |
| Rules | Allow only | Allow and deny |

- VPC peering, Transit Gateway, PrivateLink endpoints
- **Site-to-Site VPN** vs **Direct Connect**

> 💡 **Exam tip:** Direct Connect = private dedicated line; VPN = encrypted over the internet.

> 🧪 **Lab:** build a VPC with public and private subnets.

> ✅ **Checkpoint:** which is stateless, Security Group or NACL?

- [ ] Module 4 complete

---

## 🗄️ Module 5 · Storage
`🟢 Domain 3` `⏱️ 5–6 h`

### 📖 Topics
- **S3:** versioning, lifecycle, replication, encryption, 11 nines durability
- **S3 classes:** Standard → Intelligent-Tiering → Standard-IA → One Zone-IA → Glacier Instant → Glacier Flexible → Glacier Deep Archive
- **EBS** (block) · **EFS** (shared file) · **FSx** (Windows/Lustre)
- Storage Gateway, AWS Backup, Snow Family, DataSync

| Type | Service |
|:--|:--|
| 🧱 Object | S3 |
| 💽 Block | EBS |
| 📁 File | EFS / FSx |

> 💡 **Exam tip:** cheapest archive = Glacier Deep Archive; unknown access pattern = Intelligent-Tiering.

> 🧪 **Lab:** create a bucket with versioning and a Glacier lifecycle rule.

- [ ] Module 5 complete

---

## 🛢️ Module 6 · Databases
`🟢 Domain 3` `⏱️ 4–5 h`

### 📖 Topics

| Need | Service |
|:--|:--|
| Managed relational | RDS, Aurora |
| Serverless NoSQL | DynamoDB |
| In-memory cache | ElastiCache |
| Data warehouse | Redshift |
| Graph | Neptune |
| Migration | DMS |

- Analytics: Athena, Glue, EMR, Kinesis, QuickSight

> 💡 **Exam tip:** Multi-AZ = availability; read replica = performance.

> 🧪 **Lab:** compare launching an RDS instance with a DynamoDB table.

- [ ] Module 6 complete

---

## 🔐 Module 7 · Security and Identity
`🔴 Domain 2` `⏱️ 7–9 h` `🔥 highest weight`

### 📖 Topics
- **Shared Responsibility Model:** AWS = security **of** the cloud · you = security **in** the cloud
- **IAM:** users, groups, roles, policies, least privilege, MFA, Identity Center, STS, Cognito
- **Network protection:** WAF, Shield, Firewall Manager

| Threat | Service |
|:--|:--|
| Malicious activity | GuardDuty |
| Software vulnerabilities | Inspector |
| Sensitive data in S3 | Macie |
| Investigation | Detective |
| Central findings | Security Hub |

- **Encryption:** KMS, CloudHSM, ACM, Secrets Manager

> ⚠️ **Root user:** MFA on, no daily use, no access keys.

> 🧪 **Lab:** create an IAM group, user and role with a least-privilege policy and MFA.

> ✅ **Checkpoint:** who patches the OS on EC2? On RDS?

- [ ] Module 7 complete

---

## 💰 Module 8 · Pricing, Billing and Support
`🟣 Domain 4` `⏱️ 4–5 h`

### 📖 Topics
- Free Tier: always free · 12-month · short-term trials
- Outbound data is charged; inbound is free
- **Organizations:** consolidated billing, volume discounts

| Tool | Purpose |
|:--|:--|
| Pricing Calculator | Estimate future cost |
| Cost Explorer | Analyze past spend |
| Budgets | Alerts |
| Cost and Usage Report | Detailed data |

- **Support plans:** Basic · Developer · Business · Enterprise On-Ramp · Enterprise

> 💡 **Exam tip:** TAM comes with Enterprise-level plans; full Trusted Advisor checks from Business up.

> 🧪 **Lab:** create a budget; filter Cost Explorer by tag.

- [ ] Module 8 complete

---

## 🚀 Module 9 · Migration, Adoption and Innovation
`🔵 Domain 1` `🟢 Domain 3` `⏱️ 5–6 h`

### 📖 Topics
- **CAF perspectives:** Business · People · Governance · Platform · Security · Operations
- **The 7 Rs:** Retire · Retain · Rehost · Relocate · Repurchase · Replatform · Refactor
- Migration Hub, Application Migration Service, DMS, Snow Family
- **AI/ML:** SageMaker, Rekognition, Comprehend, Lex, Polly, Transcribe, Textract, Bedrock, Amazon Q

> 💡 **Exam tip:** Rehost = lift and shift; Refactor = re-architect cloud-native.

> ✅ **Checkpoint:** list the 7 Rs with one line each.

- [ ] Module 9 complete

---

## 📊 Module 10 · Monitoring, Compliance and Governance
`🔴 Domain 2` `🟢 Domain 3` `⏱️ 5–6 h`

### 📖 Topics

| Service | One-liner |
|:--|:--|
| CloudWatch | Performance metrics, alarms, logs |
| CloudTrail | Who did what, when (API audit) |
| Config | Resource configuration compliance |
| Trusted Advisor | Best-practice checks |
| Artifact | Compliance reports and agreements |
| Organizations / Control Tower | Multi-account governance |

- Health Dashboard, Systems Manager, Compute Optimizer, Audit Manager

> 🧪 **Lab:** create a CloudWatch alarm with an SNS email.

> ✅ **Checkpoint:** CloudWatch vs CloudTrail vs Config in one line each.

- [ ] Module 10 complete

---

## 🏁 Final Revision Plan

1. 📖 Re-read notes for Modules 7, 2, 5 and 4 (highest weight)
2. 🗒️ Build a one-page sheet: security services, storage, databases, support plans
3. ⏱️ Take two timed 65-question practice exams
4. 🔍 Log every miss: concept gap, misread or confused services
5. 🎯 Repeat until you score **85%+** on unseen exams

## 🗝️ Exam-Day Keywords

| Keyword in question | Think |
|:--|:--|
| Least operational overhead | Managed / serverless |
| Cost-effective | Spot, Savings Plans, storage tiering |
| Audit | CloudTrail, Config, Artifact |
| Decouple | SQS, SNS, EventBridge |

## ✅ Exam-Day Checklist
- [ ] Valid ID matching your registration name
- [ ] Flag hard questions and return to them
- [ ] Answer every question
- [ ] Keep ~10 minutes for review

**Good luck! 🍀**
