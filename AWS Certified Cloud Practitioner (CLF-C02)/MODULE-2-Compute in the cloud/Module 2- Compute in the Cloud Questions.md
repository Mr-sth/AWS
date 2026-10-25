# AWS CLF-C02 — Module 2: Compute (Enhanced Study Notes)

> **Scope:** EC2, scaling, load balancing, serverless, containers, purchasing options, plus the monitoring/governance services that show up alongside compute questions.
> **Contents:** Summary → Cheat sheets → Practice questions grouped by topic → Rapid-fire review

---

## Table of Contents

1. [Core Summary](#1-core-summary)
2. [Cheat Sheets](#2-cheat-sheets)
3. [Practice Questions](#3-practice-questions)
   - [A. EC2 Basics & Shared Responsibility](#a-ec2-basics--shared-responsibility) (Q1, 2, 16, 27)
   - [B. Scaling, Load Balancing & Availability](#b-scaling-load-balancing--availability) (Q3, 4, 5, 6, 8, 9, 10, 21, 22, 28, 35)
   - [C. Serverless & Containers](#c-serverless--containers) (Q7, 11, 17, 23, 26)
   - [D. EC2 Purchasing Options](#d-ec2-purchasing-options) (Q18, 19, 20, 24, 25)
   - [E. Monitoring, Auditing, Governance & DNS](#e-monitoring-auditing-governance--dns) (Q29–34, 36)
4. [Rapid-Fire Review](#4-rapid-fire-review)
5. [Common Exam Traps](#5-common-exam-traps)

---

## 1. Core Summary

**Compute** = the processing power used to run applications and workloads.

| Concept | Simple Meaning |
|---|---|
| **Amazon EC2** | Virtual servers in the AWS Cloud |
| **EC2 Instance** | One virtual server |
| **Instance Type** | Determines CPU, memory, storage, networking |
| **AMI** | Template used to launch an EC2 instance (OS + software + config) |
| **Amazon EC2 Auto Scaling** | Automatically adds or removes EC2 instances based on demand |
| **Elastic Load Balancing (ELB)** | Distributes incoming traffic across resources |
| **AWS Lambda** | Runs code without managing servers (serverless, event-driven) |
| **Amazon ECS** | Container orchestration service |
| **AWS Fargate** | Serverless compute engine for containers (no EC2 to manage) |

### Think of EC2 like renting a computer

> AWS provides the physical infrastructure → you get a virtual server → you configure and run your application on it.

| AWS is responsible for | You (the customer) are responsible for |
|---|---|
| Physical servers | Guest operating system (incl. patching) |
| Data centers | Applications |
| Underlying physical infrastructure | Configuration, security groups, access management |

> 💡 **Remember:** EC2 = **AWS manages the hardware, you manage the virtual server.**

### The compute spectrum (more control → less management)

```text
More control / more your responsibility                    Less control / less your responsibility
EC2  ───────────►  ECS on EC2  ───────────►  ECS + Fargate  ───────────►  Lambda
(you manage OS)   (you manage hosts)      (no servers, containers)     (no servers, just code)
```

---

## 2. Cheat Sheets

### 2.1 Scaling vs. Distributing vs. Monitoring

| Need | Service | One-liner |
|---|---|---|
| Change **how many** instances | EC2 Auto Scaling | Adds/removes instances |
| Decide **where traffic goes** | ELB | Spreads requests across healthy targets |
| **Watch** metrics / trigger alarms | CloudWatch | Metrics, logs, alarms |

**Typical architecture:**

```text
Normal traffic:   Users → ELB → EC2 | EC2
High traffic:     Users → ELB → EC2 | EC2 | EC2 | EC2 | EC2
                                ▲
              CloudWatch alarm → Auto Scaling adds instances
```

### 2.2 Horizontal vs. Vertical Scaling

| | Horizontal (scale out/in) | Vertical (scale up/down) |
|---|---|---|
| What changes | **Number** of instances | **Size** of one instance |
| Example | 1 EC2 → 3 EC2 → 10 EC2 | 2 vCPU/4 GB → 8 vCPU/16 GB |
| AWS tool | EC2 Auto Scaling | Change instance type |
| Memory trick | ↔️ More machines | ↕️ Bigger machine |

### 2.3 EC2 Purchasing Options

| Option | Best for | Key clue |
|---|---|---|
| **On-Demand** | Short-term, unpredictable workloads | No commitment, pay as you go |
| **Reserved Instances** | Steady, predictable workloads | 1- or 3-year commitment for a discount |
| **Savings Plans** | Steady usage with flexibility | Commit to a $/hour spend for 1 or 3 years |
| **Spot Instances** | Flexible, fault-tolerant, interruptible work | Lowest cost; AWS can reclaim capacity |
| **Dedicated Hosts** | Dedicated physical server requirements | Compliance / licensing tied to physical servers |

### 2.4 EC2 vs. Containers vs. Lambda

| | EC2 | ECS on EC2 | ECS + Fargate | Lambda |
|---|---|---|---|---|
| Unit | Virtual server | Container | Container | Function (code) |
| You manage servers? | **Yes** | **Yes** | No | No |
| Trigger | Always running | Always running | Always running | **Event-driven** |
| Billing | Per instance time | Per instance time | Per container resources used | Per request + duration |
| Exam clue | "Full control over OS" | "Containers + control over EC2" | "Containers + no server management" | "Event + short code + no servers" |

### 2.5 Monitoring, Auditing & Governance Services

| Service | Answers the question… | Keyword |
|---|---|---|
| **CloudWatch** | How is it performing? Alert me at 80% CPU | Metrics, logs, alarms |
| **CloudTrail** | **Who** did **what**, and **when**? | API activity, auditing |
| **AWS Config** | What did the configuration look like? Is it compliant? | Config history, rules |
| **Trusted Advisor** | How can I improve cost, security, performance, quotas? | Best-practice recommendations |
| **Service Catalog** | Which pre-approved products can developers launch? | Approved catalogs |
| **AWS Organizations** | How do I centrally manage many accounts? | OUs, SCPs |
| **Route 53** | How do users get routed to the right endpoint? | DNS, routing policies |

### 2.6 Quick Decision Tree

```text
Need to run something on AWS compute?
│
├─ Need full OS control? ─────────────────────────────► EC2
├─ Running containers?
│   ├─ Don't want to manage servers ──────────────────► ECS + Fargate
│   └─ Want control of the EC2 hosts ─────────────────► ECS on EC2
└─ Event-triggered short code, no servers ────────────► Lambda

Need more capacity?
├─ Add more instances ────────────────────────────────► Horizontal (Auto Scaling)
└─ Make one instance bigger ──────────────────────────► Vertical

Need to cut EC2 cost?
├─ Interruptible / flexible ──────────────────────────► Spot
├─ Predictable, 1–3 years ────────────────────────────► Reserved / Savings Plans
└─ Short-term, unpredictable ─────────────────────────► On-Demand
```

---

## 3. Practice Questions

**Format:** each question shows the scenario, options (✅ marks the correct one), your answer where recorded, why, the exam clue, and a memory hook.

> 📝 **Numbering note:** Questions 12–15 are not in the source file. Add them here if you have them.

---

### A. EC2 Basics & Shared Responsibility

#### Q1 — What is an EC2 instance?

What is an **Amazon EC2 instance**?

- A. A physical server located in an AWS data center
- B. A virtual server that runs in the AWS Cloud ✅
- C. A database used to store application data
- D. A network connection between two AWS Regions

**Why:** An EC2 instance is a **virtual computer in AWS**. You choose CPU, memory, storage, OS, and network configuration.

> 💡 **Remember:** Laptop = physical computer → **EC2 instance = virtual computer in AWS**

---

#### Q2 — Who manages the guest OS?

A company launches an EC2 instance and needs to install and manage the operating system and applications running on it. **Who is primarily responsible for managing the guest operating system?**

- A. AWS
- B. The customer ✅
- C. The internet service provider
- D. The AWS Region

**Why:** AWS manages the **underlying physical infrastructure**; the customer manages the **guest OS, applications, and configuration**. This is the **Shared Responsibility Model**.

> 💡 **Remember:** 🏢 AWS → Physical infrastructure | 💻 Customer → OS + Applications + Configuration

---

#### Q16 — Full control over OS and software

A company wants to launch a virtual server in AWS and have **full control over the operating system, installed software, and configuration**. Which service?

- A. AWS Lambda
- B. Amazon EC2 ✅
- C. Amazon ECS
- D. Amazon S3

**Your answer:** B ✅ Correct

**Why:** EC2 provides virtual servers where you control the OS, software, and configuration.

**Exam clue:** "Virtual server" + "control over OS" → **Amazon EC2**

---

#### Q27 — Guest OS patching scenario

A company runs an application on EC2. The company installs security patches on the guest OS, while AWS maintains the physical host and hardware. Which concept does this demonstrate?

- A. Fault tolerance
- B. The AWS Shared Responsibility Model ✅
- C. Elasticity
- D. The AWS Well-Architected Framework

**Your answer:** B ✅ Correct

**Why:**
- **AWS — security *OF* the cloud:** physical facilities, hardware, underlying infrastructure.
- **Customer — security *IN* the cloud:** guest OS patching on EC2, application configuration, access management.
- **Fault tolerance:** continuing to operate despite failures.
- **Elasticity:** adjusting resources to match demand.
- **Well-Architected Framework:** design guidance for cloud workloads.

**Exam tip:** AWS secures the infrastructure **OF** the cloud; customers secure their workloads **IN** the cloud. The exact split varies by service (e.g., you patch EC2 yourself, but AWS patches the OS under Lambda).

---

### B. Scaling, Load Balancing & Availability

#### Q3 — Adding more instances

An application runs on one EC2 instance. During busy periods it needs more computing capacity. **Which approach means adding more EC2 instances?**

- A. Vertical scaling
- B. Horizontal scaling ✅
- C. Data encryption
- D. Load balancing

**Why:** Horizontal scaling = **add more machines** (1 → 3 → 10). Vertical scaling = make one machine bigger.

> 💡 **Remember:** ↔️ Horizontal = **More machines** | ↕️ Vertical = **Bigger machine**

---

#### Q4 — Resizing an instance

An application runs on **2 vCPUs / 4 GB RAM**. The company changes it to **8 vCPUs / 16 GB RAM**. What type of scaling?

- A. Horizontal scaling
- B. Vertical scaling ✅
- C. Automatic scaling
- D. Geographic scaling

**Why:** The CPU and RAM of the **same instance** increased → vertical scaling.

> 💡 **Remember:** ↔️ Horizontal = **More instances** | ↕️ Vertical = **Bigger instance**

---

#### Q5 — Automatic add/remove for a sale

A website normally runs on **2 instances**, needs **10** during a sale, then returns to 2. **Which service automatically adds or removes EC2 instances based on demand?**

- A. Amazon EC2 Auto Scaling ✅
- B. Amazon S3
- C. AWS Lambda
- D. Amazon RDS

**Why:** Auto Scaling adjusts the **number of instances** to match demand (2 → 10 → 2). This is mainly horizontal scaling.

> 💡 **Remember:** Auto Scaling = **Automatically add/remove**

---

#### Q6 — Distributing requests

Multiple EC2 instances run the same web app. The company wants requests distributed so no single instance gets all the traffic. Which service?

- A. Amazon EC2 Auto Scaling
- B. Elastic Load Balancing (ELB) ✅
- C. AWS Lambda
- D. Amazon S3

**Why:** ELB spreads incoming traffic across multiple instances. Auto Scaling changes the *number* of instances; ELB distributes *traffic*.

> 💡 **Remember:** Auto Scaling = **How many instances?** | ELB = **Where does the traffic go?**

---

#### Q8 — Scale out *and* receive traffic

A company wants **more EC2 instances launched when demand increases**, and those instances should also receive incoming traffic. Which combination?

- A. EC2 Auto Scaling + Elastic Load Balancing ✅
- B. AWS Lambda + Amazon S3
- C. Amazon RDS + Amazon DynamoDB
- D. Amazon S3 + Elastic Load Balancing

**Why:** They work as a team: **Auto Scaling** decides how many instances; **ELB** distributes traffic across them.

```text
Normal:  Users → ELB → EC2 | EC2
High:    Users → ELB → EC2 | EC2 | EC2 | EC2 | EC2
```

> 💡 **Remember:** Auto Scaling = **How many?** | ELB = **Where does traffic go?**

---

#### Q9 — Add instances instead of a bigger one

A web app on a single EC2 instance becomes slow under high traffic. The company wants to **add instances** rather than enlarge the existing one.

- A. Increase the instance size to provide more CPU and memory
- B. Use EC2 Auto Scaling to launch additional instances based on demand ✅
- C. Use Amazon RDS to increase the application's compute capacity
- D. Use AWS Lambda to vertically scale the existing EC2 instance

**Why:** Adding instances = horizontal scaling; doing it automatically = Auto Scaling. **A** is vertical scaling.

> 💡 **Remember:** More instances = **Horizontal** | Bigger instance = **Vertical**

---

#### Q10 — Uneven utilization

Some instances receive far more requests than others during a spike. The company wants to distribute incoming traffic across the instances.

- A. Amazon EC2 Auto Scaling
- B. Elastic Load Balancing (ELB) ✅
- C. AWS Lambda
- D. Amazon CloudWatch

**Why:**
- **ELB** → distributes incoming traffic
- **Auto Scaling** → adds/removes instances
- **CloudWatch** → monitors metrics
- **Lambda** → runs code without servers

> 💡 **Remember:** ELB = **Distribute traffic** | Auto Scaling = **Adjust instances**

---

#### Q21 — Distribute traffic *and* scale automatically

Instances become overloaded while others sit idle. The company wants to distribute requests across **healthy** instances and automatically add/remove instances. Which solution?

- A. CloudWatch to distribute traffic and Lambda to add instances
- B. ELB to distribute traffic and EC2 Auto Scaling to adjust the number of instances ✅
- C. Route 53 to distribute traffic and S3 to scale instances
- D. Lambda to distribute traffic and ECS to increase EC2 instances

**Result:** ✅ Correct

**Why:**
- **ELB:** distributes traffic across healthy targets.
- **EC2 Auto Scaling:** adds/removes instances per scaling policies.
- **CloudWatch:** monitors and alarms; doesn't distribute traffic.
- **Route 53:** DNS routing; doesn't create or resize instances.

**Exam tip:** ELB distributes traffic; Auto Scaling adjusts instance count. Together they improve availability and scalability.

---

#### Q22 — Survive an Availability Zone failure

An application must stay available even if an entire **Availability Zone** becomes unavailable.

- A. Multiple EC2 instances in the same AZ with vertical scaling
- B. EC2 instances across multiple AZs with Elastic Load Balancing ✅
- C. Application backups in S3 in the same AZ
- D. Increase CPU and memory of a single instance

**Your answer:** B ✅ Correct

**Key terms:**
- **Availability Zone (AZ):** an isolated location within a Region.
- **High availability:** staying accessible when a component fails.
- **Fault tolerance:** continuing to operate despite failures.

**Why the others fail:**
- **A:** same AZ = same single point of failure.
- **C:** backups alone don't keep the app running.
- **D:** vertical scaling doesn't protect against AZ failure.

**Exam tip:** "Entire AZ unavailable" → deploy across **multiple AZs**.

---

#### Q28 — Choosing the scaling approach

During peak hours CPU utilization rises. The company wants to add more instances rather than upgrade existing ones.

- A. Vertical scaling
- B. Horizontal scaling ✅
- C. Manual backup
- D. Fault tolerance

**Your answer:** B ✅ Correct

**Notes:**
- **Horizontal (scale out/in):** add/remove instances.
- **Vertical (scale up/down):** resize an existing instance.
- **EC2 Auto Scaling** can automate horizontal scaling; **ELB** distributes the traffic.

**Exam tip:** More servers = horizontal. One bigger server = vertical.

**Important:** Horizontal scaling generally requires an application architecture that works across multiple instances (e.g., stateless).

---

#### Q35 — Auto add/remove on CPU

An application on EC2 should automatically gain instances when CPU rises and lose them when demand falls.

- A. AWS CloudTrail
- B. Amazon EC2 Auto Scaling ✅
- C. AWS Config
- D. AWS Trusted Advisor

**Your answer:** B ✅ Correct

**Notes:**
- **EC2 Auto Scaling:** adjusts instance count per scaling policies.
- **CloudWatch:** monitors CPU and can trigger alarms.
- **CloudTrail:** records API activity. **Config:** tracks configuration/compliance. **Trusted Advisor:** best-practice recommendations.

**Exam tip:** Automatically adding/removing EC2 instances based on demand → **EC2 Auto Scaling**.

**Important:** Auto Scaling needs scaling policies and configuration; it doesn't scale every workload automatically out of the box.

---

### C. Serverless & Containers

#### Q7 — Code on an event, no servers

A developer has a small piece of code that should run when an event occurs and does **not** want to provision or manage servers.

- A. Amazon EC2
- B. Amazon RDS
- C. AWS Lambda ✅
- D. Amazon ECS

**Why:** You provide the **code**; AWS manages the underlying compute. **EC2** → you manage the server. **Lambda** → you focus on code.

> 💡 **Remember:** ⚡ Lambda = **Run code without managing servers**

---

#### Q11 — Process an S3 upload

An application must process an image whenever a user uploads it to S3. The code runs for a few seconds. The company doesn't want to manage servers and wants to pay only when code runs.

- A. Amazon EC2
- B. Amazon ECS
- C. AWS Lambda ✅
- D. Amazon RDS

**Why:**
- **Event-driven:** triggered by an S3 upload
- **Short-running:** a few seconds
- **Serverless:** no provisioning or management
- **Pay for execution:** billed on Lambda usage

```text
S3 Upload → Lambda triggered → Process image → Function finishes
```

**EC2:** virtual server, you manage OS/software/config.
**Lambda:** you provide the function; AWS runs it when needed.

> 💡 **CLF-C02 shortcut:** **Event + short-running code + no server management = Lambda**

**Important:** Lambda is **not** an EC2 instance. They are separate compute services.

---

#### Q17 — Containers without managing servers

A company wants to run containers but **does not want to manage the underlying servers or EC2 instances**.

- A. Amazon EC2
- B. Amazon ECS with AWS Fargate ✅
- C. AWS Lambda
- D. Amazon EC2 Auto Scaling

**Your answer:** B ✅ Correct

**Why:**
- **ECS** → manages and runs your containers
- **Fargate** → runs them without managing EC2 servers
- **EC2** → you manage the server
- **Lambda** → runs functions; in the usual CLF-C02 framing it isn't the answer for general containerized apps

**Exam clue:** "Containers" + "do not want to manage servers" → **ECS with Fargate**

---

#### Q23 — Serverless, S3-triggered, pay for compute used

Code should execute only when a file is uploaded to S3, with no server provisioning, paying for compute time used.

- A. Amazon EC2
- B. Amazon ECS on Amazon EC2
- C. AWS Lambda ✅
- D. Amazon RDS

**Your answer:** C ✅ Correct

**Notes:**
- **Lambda:** event-driven, no servers; billed on requests and execution duration.
- **EC2:** virtual servers you manage.
- **ECS on EC2:** manages containers, but you manage the EC2 hosts.
- **RDS:** managed relational database, not a code-execution service.

**Exam tip:** Serverless + event-triggered + no server management → **Lambda**.

---

#### Q26 — Containers, AWS manages infrastructure

A company wants to run containers without managing the underlying EC2 instances, focusing only on deploying its containerized app.

- A. Amazon EC2, because AWS manages the guest OS and application configuration
- B. Amazon ECS with AWS Fargate, because it runs containers without requiring the company to manage underlying servers ✅
- C. Amazon EC2 Auto Scaling, because it automatically manages container images
- D. AWS Lambda, because it runs every type of long-running container workload without limitations

**Your answer:** B ✅ Correct

**Notes:**
- **ECS:** container orchestration. **Fargate:** no EC2 hosts to manage.
- **EC2:** you manage the guest OS (A is wrong).
- **EC2 Auto Scaling:** adjusts instance count, doesn't manage images (C is wrong).
- **Lambda:** event-driven, with execution limits (D's "without limitations" is wrong).

**Exam tip:** Containers + no server management → **ECS with Fargate**. Containers + control over EC2 → **ECS on EC2**.

---

### D. EC2 Purchasing Options

#### Q18 — Interruptible, lowest cost

A workload can be interrupted at any time, isn't time-sensitive, and can restart from the beginning. The company wants to **minimize cost**.

- A. On-Demand Instances
- B. Reserved Instances
- C. Spot Instances ✅
- D. Dedicated Hosts

**Your answer:** C ✅ Correct

**Why:** Spot uses unused EC2 capacity at a **much lower price** than On-Demand, but AWS can interrupt it when capacity is needed.

**Exam clue:** Flexible + can be interrupted + lowest cost → **Spot**

---

#### Q19 — Predictable for 3 years

A workload runs continuously and predictably for **3 years**; the company will make a **long-term commitment** to cut costs versus On-Demand.

- A. Spot Instances
- B. On-Demand Instances
- C. Reserved Instances ✅
- D. Dedicated Hosts

**Your answer:** C ✅ Correct

**Why:** Reserved Instances suit **predictable, steady workloads** with a **1- or 3-year** commitment in exchange for a discount.

**Exam clue:** Predictable workload + long-term commitment → **Reserved Instances**

---

#### Q20 — Short-term, unpredictable

An EC2 workload may run for only a few days, is unpredictable, and the company wants **no long-term commitment**.

- A. Reserved Instances
- B. Spot Instances
- C. On-Demand Instances ✅
- D. Dedicated Hosts

**Your answer:** C ✅ Correct

**Why:** On-Demand fits no commitment, flexible usage, and short-term or unpredictable workloads.

**Exam clue:** Short-term / unpredictable / no commitment → **On-Demand**

---

#### Q24 — Batch job, minimize cost

A batch-processing workload can be interrupted and restarted later. The company wants to minimize compute cost and can tolerate interruption when AWS needs capacity back.

- A. On-Demand Instances
- B. Reserved Instances
- C. Spot Instances ✅
- D. Dedicated Hosts

**Your answer:** C ✅ Correct

**Notes:**
- **Spot:** spare capacity at a discount; can be interrupted.
- **On-Demand:** flexible, no long-term commitment.
- **Reserved:** discount for a committed, consistent usage pattern.
- **Dedicated Hosts:** physical servers dedicated to one customer.

**Exam tip:** Flexible + interruptible + cost-sensitive → **Spot**.

---

#### Q25 — Business-critical, predictable, 3 years

A business-critical EC2 workload is predictable and runs continuously for three years. The company wants lower costs than On-Demand.

- A. Spot Instances, because they always provide the lowest cost without interruption
- B. On-Demand Instances, because they require a three-year commitment
- C. Reserved Instances, because the company has predictable long-term usage ✅
- D. Dedicated Hosts, because they automatically scale based on demand

**Your answer:** C ✅ Correct

**Why the others are wrong:**
- **A:** Spot *can* be interrupted.
- **B:** On-Demand has *no* commitment.
- **D:** Dedicated Hosts don't auto-scale.

**Exam tip:** Predictable long-term usage → **Reserved**. Flexible → **On-Demand**. Interruptible and cost-sensitive → **Spot**.

**Important:** Reserved Instances are a **billing commitment**, not a guarantee of auto-scaling or of capacity in every configuration.

---

### E. Monitoring, Auditing, Governance & DNS

#### Q29 — Alarm on high CPU

A company wants an alarm when CPU utilization exceeds 80% for a sustained period so the operations team can investigate.

- A. AWS CloudTrail
- B. Amazon CloudWatch ✅
- C. AWS Config
- D. AWS Trusted Advisor

**Your answer:** B ✅ Correct

**Exam tip:** CPU metrics and threshold alarms → **CloudWatch**. API activity → CloudTrail. Configuration history/compliance → Config.

---

#### Q30 — Who changed what, and when

A company wants to record who changed AWS resources, which API actions were performed, and when, for security auditing.

- A. Amazon CloudWatch
- B. AWS CloudTrail ✅
- C. AWS Auto Scaling
- D. AWS Trusted Advisor

**Your answer:** B ✅ Correct

**Exam tip:** **Who did what, and when?** → CloudTrail. **How is it performing?** → CloudWatch.

**Important:** CloudTrail *records* activity; it doesn't prevent unauthorized actions. Use IAM policies to control permissions.

---

#### Q31 — Recommendations across categories

A company wants to reduce costs and get recommendations on security, service quotas, and performance.

- A. AWS CloudTrail
- B. Amazon CloudWatch
- C. AWS Trusted Advisor ✅
- D. Amazon EC2 Auto Scaling

**Your answer:** C ✅ Correct

**Notes:** Trusted Advisor covers cost optimization, security, performance, fault tolerance, and service quotas.

**Exam tip:** Best-practice checks across multiple categories → **Trusted Advisor**.

**Important:** It *recommends*; it doesn't implement every recommendation. Available checks depend on your AWS Support plan.

---

#### Q32 — Preapproved infrastructure catalog

Developers should launch only approved infrastructure configurations (e.g., EC2, databases) from a catalog of preapproved products.

- A. AWS Service Catalog ✅
- B. AWS CloudTrail
- C. Amazon CloudWatch
- D. AWS Trusted Advisor

**Your answer:** A ✅ Correct

**Exam tip:** Preapproved products + controlled self-service → **Service Catalog**.

**Important:** Service Catalog standardizes deployments and applies governance; it isn't a monitoring or auditing service.

---

#### Q33 — Multi-account governance

A company with multiple AWS accounts wants to centrally manage security policies and group accounts for easier governance.

- A. AWS Organizations ✅
- B. Amazon CloudWatch
- C. AWS CloudTrail
- D. AWS Lambda

**Your answer:** A ✅ Correct

**Notes:**
- **Organizations:** centrally manages multiple accounts.
- **OUs:** group accounts for management.
- **SCPs:** set the *maximum* permissions available to accounts; they don't grant permissions.

**Exam tip:** Multiple accounts + centralized governance + OUs → **Organizations**.

**Important:** SCPs don't replace IAM. An action must be allowed by permission policies *and* not blocked by an SCP.

---

#### Q34 — Configuration history and compliance

A company wants to track configuration changes over time and evaluate whether resources comply with internal rules.

- A. AWS CloudTrail
- B. AWS Config ✅
- C. Amazon CloudWatch
- D. AWS Trusted Advisor

**Your answer:** *(not recorded — add yours here)*

**Notes:**
- **Config:** records configuration changes and evaluates resources against rules.
- **CloudTrail:** who performed an action and when.
- **CloudWatch:** metrics, logs, alarms.
- **Trusted Advisor:** improvement recommendations.

**Exam tip:** Configuration history + compliance → **Config**. User activity + API auditing → **CloudTrail**.

**Memory trick:** **Config** = configuration; **CloudTrail** = activity trail.

---

#### Q36 — DNS-based routing across Regions

A company deploys in multiple Regions to reduce latency and wants users directed to an appropriate endpoint using DNS routing policies.

- A. Amazon CloudWatch
- B. AWS CloudTrail
- C. Amazon Route 53 ✅
- D. Amazon EC2 Auto Scaling

**Your answer:** C ✅ Correct

**Notes:**
- **Route 53:** DNS service with domain registration, routing, and health checks.
- **Latency-based routing:** sends users to the endpoint with the best network latency.

**Exam tip:** Domain names, DNS, DNS routing policies → **Route 53**.

**Important:** Route 53 directs DNS queries; it doesn't deploy application resources across Regions.

---
 
#### Q37 — Migrate a database with minimal downtime
 
A company wants to migrate its relational database from an on-premises data center to Amazon RDS. The company wants to **minimize application downtime** during the migration. Which AWS service is designed to help migrate databases to AWS?
 
- A. AWS DataSync
- B. AWS Database Migration Service (AWS DMS) ✅
- C. AWS CloudTrail
- D. Amazon Route 53
**Your answer:** B ✅ Correct
 
**Why:**
- **AWS DMS:** Migrates databases to AWS and supports ongoing replication of changes to help minimize downtime.
- **AWS DataSync:** Transfers files and object data between supported storage locations.
- **AWS CloudTrail:** Records AWS account activity and API calls.
- **Amazon Route 53:** Provides DNS and routing services.
**Exam tip:** Database migration → **AWS DMS**. File and object data transfer → **AWS DataSync**.
 
**Important:** AWS DMS supports many migration scenarios, but schema conversion between different database engines may require the AWS Schema Conversion Tool (AWS SCT) or other conversion capabilities.
 
---
 
#### Q38 — Managed relational database
 
A company wants to run a relational database in AWS **without managing the underlying server hardware** or performing routine maintenance such as automated backups and software patching. Which AWS service best meets these requirements?
 
- A. Amazon EC2
- B. Amazon RDS ✅
- C. Amazon S3
- D. Amazon DynamoDB
**Your answer:** B ✅ Correct
 
**Why:**
- **Amazon RDS:** A managed relational database service. AWS manages the underlying infrastructure and supports routine maintenance tasks.
- **Amazon EC2:** Provides virtual servers; customers manage the guest OS and any database software they install.
- **Amazon S3:** Object storage, not a relational database.
- **Amazon DynamoDB:** A managed NoSQL database service.
**Exam tip:** Managed relational database → **Amazon RDS**. Managed NoSQL database → **Amazon DynamoDB**.
 
**Important:** Automated backups, maintenance windows, and patching behavior depend on the RDS configuration and database engine. Customers still manage database access, data, and many database-level settings.
 
---
 
#### Q39 — Key-value NoSQL at massive scale
 
A company needs a database that can handle **millions of requests per second** with **single-digit millisecond** performance. The application stores data as **key-value pairs** and requires a **fully managed NoSQL** database. Which AWS service is the best choice?
 
- A. Amazon RDS
- B. Amazon Redshift
- C. Amazon DynamoDB ✅
- D. Amazon Aurora
**Your answer:** C ✅ Correct
 
**Why:**
- **Amazon DynamoDB:** A fully managed NoSQL database supporting key-value and document data models with low-latency performance at scale.
- **Amazon RDS:** A managed relational database service.
- **Amazon Redshift:** A data warehouse service designed for analytics.
- **Amazon Aurora:** A relational database engine compatible with MySQL and PostgreSQL.
**Exam tip:** Key-value or document data + highly scalable, low-latency NoSQL → **DynamoDB**.
 
**Important:** DynamoDB is not a relational database; its data model and query capabilities differ from RDS and Aurora.
 
---
 
#### Q40 — Data warehouse for analytics
 
A company wants to analyze large amounts of historical sales data using **complex SQL queries**. The data is stored in a **data warehouse**, and the company needs fast analytical queries across large datasets. Which AWS service is the best choice?
 
- A. Amazon DynamoDB
- B. Amazon Redshift ✅
- C. Amazon EC2 Auto Scaling
- D. Amazon Route 53
**Your answer:** B ✅ Correct
 
**Why:**
- **Amazon Redshift:** A cloud data warehouse designed for large-scale analytics and complex SQL queries.
- **Amazon DynamoDB:** A managed NoSQL database for key-value and document data.
- **Amazon EC2 Auto Scaling:** Adjusts the number of EC2 instances based on configured policies.
- **Amazon Route 53:** Provides DNS and routing services.
**Exam tip:** Large-scale analytics and data warehousing → **Amazon Redshift**. Transactional relational workloads → RDS or Aurora. Key-value NoSQL workloads → DynamoDB.
 
**Important:** A data warehouse is optimized for analytical queries, not primarily for the day-to-day transactional operations of an application.
 
---
 
#### Q41 — MySQL/PostgreSQL-compatible managed database
 
A company has an application that requires a relational database **compatible with MySQL or PostgreSQL**. The company wants improved performance and availability compared with a typical self-managed database, while **reducing administrative overhead**. Which AWS service is the best choice?
 
- A. Amazon DynamoDB
- B. Amazon Aurora ✅
- C. Amazon Redshift
- D. Amazon S3
**Your answer:** B ✅ Correct
 
**Why:**
- **Amazon Aurora:** A relational database engine compatible with MySQL and PostgreSQL, designed for performance and availability.
- **Amazon DynamoDB:** A managed NoSQL database.
- **Amazon Redshift:** A data warehouse for analytical workloads.
- **Amazon S3:** Object storage for files and other objects.
**Exam tip:** MySQL/PostgreSQL compatibility + managed relational database focused on performance and availability → consider **Amazon Aurora**.
 
**Important:** Aurora reduces infrastructure administration, but customers remain responsible for database configuration, access controls, and application-level data management.
 
---
 
#### Q42 — Distribute traffic across Availability Zones
 
A company runs a website on a single large EC2 instance. Traffic has grown, and the company wants to improve availability by running **multiple instances across different Availability Zones**. Which additional service is best suited to **distribute incoming web traffic** across these instances?
 
- A. AWS CloudTrail
- B. Elastic Load Balancing (ELB) ✅
- C. AWS Config
- D. AWS Trusted Advisor
**Your answer:** B ✅ Correct
 
**Why:**
- **ELB:** Distributes incoming traffic across healthy targets, including EC2 instances in multiple Availability Zones.
- **EC2 Auto Scaling:** Automatically adjusts the number of EC2 instances based on configured scaling policies.
- **AWS CloudTrail:** Records account activity and API calls.
- **AWS Config:** Tracks resource configuration changes and evaluates compliance.
- **AWS Trusted Advisor:** Provides recommendations for AWS best practices.
**Exam tip:** Distributing traffic → **ELB**. Adjusting instance count → **EC2 Auto Scaling**. Surviving an AZ failure → design across **multiple AZs**.
 
**Important:** ELB alone does not guarantee high availability; the application must have healthy targets in other Availability Zones and be configured appropriately.
 
---
 
#### Q43 — Full control over the operating system
 
A company wants to run an application that requires **complete control over the operating system**, including installing custom software and configuring system-level settings. The company is willing to manage the OS and application environment itself. Which AWS service is the best choice?
 
- A. AWS Lambda
- B. Amazon EC2 ✅
- C. AWS Fargate
- D. Amazon RDS
**Your answer:** B ✅ Correct
 
**Why:**
- Amazon EC2 provides virtual servers.
- The **customer** manages the guest OS, software, and system-level settings.
- **AWS** manages the physical servers and underlying infrastructure.
**Exam tip:** When a question emphasizes **control over the operating system**, Amazon EC2 is usually the best choice.
 
---
 
#### Q44 — Scale automatically *and* distribute traffic
 
A company runs a web application on Amazon EC2 instances. Traffic increases significantly during business hours and decreases overnight. The company wants to **automatically adjust the number of EC2 instances based on demand** and **distribute incoming requests across healthy instances**. Which solution best meets these requirements?
 
- A. Use Amazon CloudWatch to distribute traffic and AWS CloudTrail to add or remove EC2 instances.
- B. Use Amazon EC2 Auto Scaling to adjust the number of instances and Elastic Load Balancing (ELB) to distribute incoming traffic. ✅
- C. Use ELB to automatically launch instances and EC2 Auto Scaling to distribute traffic.
- D. Use AWS Lambda to manage EC2 instance capacity and Amazon Route 53 to check instance health.
**Your answer:** B ✅ Correct
 
**Why:**
- **Amazon EC2 Auto Scaling:** Automatically adds or removes EC2 instances as demand changes.
- **Elastic Load Balancing:** Distributes incoming requests across healthy instances.
- **Amazon CloudWatch:** Monitors metrics and raises alarms; it does not distribute traffic.
- **AWS CloudTrail:** Records AWS API activity; it does not add or remove instances.
**Exam tip:** Option C swaps the two roles — a classic trap. **Auto Scaling = how many instances. ELB = where traffic goes.**
 
---
 
#### Q45 — Short code, no servers to manage
 
A company runs an application on Amazon EC2 that needs to process uploaded files. The processing code runs for only a few seconds each time. The company wants to **minimize operational overhead** and **avoid managing servers**. Which AWS service is the best choice?
 
- A. Amazon EC2, because virtual servers automatically eliminate server management.
- B. Amazon EC2 Auto Scaling, because it runs code without requiring compute instances.
- C. AWS Lambda, because it runs code in response to events without requiring the company to manage servers. ✅
- D. Amazon ECS on Amazon EC2, because containers eliminate the need to manage the underlying servers.
**Your answer:** C ✅ Correct
 
**Why:**
- **AWS Lambda:** Runs code without requiring you to provision or manage servers. Well suited to event-driven tasks, such as processing files uploaded to Amazon S3.
- **Amazon EC2:** Provides virtual servers, but you manage the guest OS and application environment.
- **Amazon EC2 Auto Scaling:** Adjusts the number of EC2 instances; it does not run code as a serverless function.
- **Amazon ECS on Amazon EC2:** Runs containerized applications, but you still manage the underlying EC2 instances.
| Service | Key responsibility |
|---|---|
| AWS Lambda | Runs code without server management |
| Amazon EC2 | Virtual servers |
| Amazon EC2 Auto Scaling | Automatically adjusts EC2 instance capacity |
| Amazon ECS | Manages containerized applications |
 
**Exam tip:** If a question emphasizes **no server management** and **short, event-driven code**, think **AWS Lambda**.
 
---
 
#### Q46 — Send traffic only to healthy instances
 
A company runs a customer-facing application on EC2 instances. The application must remain available even if **one instance becomes unhealthy**, and incoming traffic must be sent **only to healthy instances**. Which solution best meets this requirement?
 
- A. Use Amazon EC2 Auto Scaling to distribute incoming requests among healthy instances.
- B. Use Elastic Load Balancing (ELB) with health checks to route traffic to healthy instances. ✅
- C. Use AWS CloudTrail to detect unhealthy instances and redirect incoming traffic.
- D. Use AWS Lambda to replace the load balancer and distribute requests.
**Your answer:** B ✅ Correct
 
**Why:**
- **ELB:** Distributes incoming requests across registered targets.
- **Health checks:** Let ELB identify unhealthy targets so it stops routing requests to them.
- **EC2 Auto Scaling:** Can replace unhealthy instances and adjust the number of instances, but does not distribute incoming traffic.
- **AWS CloudTrail:** Records AWS API activity; it is not a traffic-routing or health-checking service.
| Requirement | AWS service |
|---|---|
| Distribute incoming traffic | Elastic Load Balancing (ELB) |
| Route traffic to healthy targets | ELB health checks |
| Replace unhealthy EC2 instances | Amazon EC2 Auto Scaling |
| Record AWS API activity | AWS CloudTrail |
 
**Exam tip:** **ELB routes traffic; EC2 Auto Scaling manages instance capacity.**
 
---
 
#### Q47 — Containers without managing servers
 
A company needs to run a containerized application on AWS. It wants **AWS to manage the underlying servers** so the development team can focus on deploying and running containers. Which solution best meets this requirement?
 
- A. Run containers on Amazon EC2 and manually manage the operating systems and container infrastructure.
- B. Use Amazon ECS with AWS Fargate to run containers without managing the underlying servers. ✅
- C. Use Amazon EC2 Auto Scaling to build and execute container images without a container service.
- D. Use AWS Lambda to host and manage any long-running containerized application without limitations.
**Your answer:** B ✅ Correct
 
**Why:**
- **Amazon ECS (Elastic Container Service):** Orchestrates and manages containerized applications.
- **AWS Fargate:** Provides serverless compute for containers, eliminating the need to provision or manage the underlying EC2 instances.
- **Option A:** You manage the OS and container infrastructure yourself.
- **Option C:** Auto Scaling adjusts instance capacity; it is not a container service.
- **Option D:** "Any … without limitations" is absolute wording — a red flag.
**Exam tip:** Containers + no server management → **ECS with Fargate**.
 
---
 
#### Q48 — Add/remove instances for traffic spikes
 
A company runs an application on EC2 instances with predictable demand during business hours but occasional unexpected traffic spikes. It wants to **automatically add instances when demand increases and remove them when demand decreases**. Which AWS service should it use?
 
- A. Elastic Load Balancing (ELB), because it automatically launches and terminates EC2 instances based on demand.
- B. Amazon EC2 Auto Scaling, because it adjusts the number of EC2 instances according to scaling policies. ✅
- C. AWS CloudTrail, because it monitors CPU utilization and adjusts EC2 capacity.
- D. Amazon Route 53, because it scales EC2 instances based on DNS requests.
**Your answer:** B ✅ Correct
 
**Why:**
- **Amazon EC2 Auto Scaling:** Automatically adds or removes EC2 instances based on configured scaling policies and demand.
- **ELB:** Distributes incoming traffic across registered targets.
- **AWS CloudTrail:** Records AWS API activity.
- **Amazon Route 53:** Provides DNS and domain-routing services.
| Requirement | AWS service |
|---|---|
| Automatically add or remove EC2 instances | EC2 Auto Scaling |
| Distribute incoming traffic | ELB |
| Record AWS API activity | CloudTrail |
| Route users through DNS | Route 53 |
 
---
 
#### Q49 — CPU-intensive scientific calculations
 
A company needs an EC2 instance type for an application that performs **complex scientific calculations** and requires **high CPU performance**. The application does not need unusually large amounts of memory. Which EC2 instance family is the best fit?
 
- A. Memory optimized
- B. Storage optimized
- C. Compute optimized ✅
- D. Accelerated computing
**Your answer:** C ✅ Correct
 
**Why:**
- **Compute optimized:** Designed for CPU-intensive workloads, such as scientific calculations and batch processing.
- **Memory optimized:** Designed for workloads that process large datasets in memory.
- **Storage optimized:** Designed for workloads requiring high storage performance and throughput.
- **Accelerated computing:** Uses hardware accelerators, such as GPUs, for workloads like machine learning and graphics processing.
| EC2 instance family | Best suited for |
|---|---|
| General purpose | Balanced CPU, memory, and networking |
| Compute optimized | High CPU performance |
| Memory optimized | Large in-memory datasets |
| Storage optimized | High-speed local storage access |
| Accelerated computing | GPUs and specialized hardware acceleration |
 
---
 
#### Q50 — Very fast access to local disks
 
A company runs an application that needs **very high, low-latency access to large amounts of data stored on local disks** attached to its EC2 instances. Which EC2 instance family is the best fit?
 
- A. General purpose
- B. Compute optimized
- C. Storage optimized ✅
- D. Memory optimized
**Your answer:** C ✅ Correct
 
**Why:**
- **Storage optimized:** Designed for workloads requiring high storage performance, high I/O throughput, and low-latency access to locally stored data.
- **General purpose:** Balanced CPU, memory, and networking.
- **Compute optimized:** CPU-intensive workloads.
- **Memory optimized:** Large in-memory datasets.
**Exam tip:** CPU → Compute optimized | RAM → Memory optimized | Local disk speed → **Storage optimized** | GPUs → Accelerated computing.
 
---
 
#### Q51 — 1-year commitment with flexibility
 
A company needs to run a workload **for one year**, continuously, and wants to **reduce EC2 costs compared with On-Demand**. It also wants **flexibility to change instance families, sizes, and AWS Regions**. Which purchasing option best meets these requirements?
 
- A. Spot Instances
- B. On-Demand Instances
- C. Standard Reserved Instances
- D. Compute Savings Plans ✅
**Your answer:** D ✅ Correct
 
**Why:** Compute Savings Plans offer lower compute costs in exchange for a commitment to a consistent amount of usage (measured in **USD per hour**) over a **1- or 3-year** term. They provide flexibility across:
- EC2 instance families and sizes
- AWS Regions
- Operating systems and tenancy
- Amazon EC2, AWS Lambda, and AWS Fargate usage
**Why the other options are wrong:**
- **A. Spot:** Best for workloads that can tolerate interruptions.
- **B. On-Demand:** No commitment, but generally higher prices.
- **C. Standard Reserved Instances:** Offer discounts but have more limited flexibility, particularly for instance family and Region.
**Exam tip:** Compute Savings Plans offer more flexibility than EC2 Instance Savings Plans. Both generally require a 1- or 3-year commitment.
 
---
 
#### Q52 — EC2 database: persistent block storage and backups
 
A company runs a database on an EC2 instance. It requires **persistent, low-latency block storage**. The company must be able to **stop and restart the instance without losing the database files**, and wants **point-in-time backups** of the storage volume. Which solution best meets these requirements?
 
- A. Store the database files on an EC2 instance store volume and back up the EC2 instance configuration.
- B. Use an Amazon EBS volume for the database files and create EBS snapshots for backups. ✅
- C. Store the database files in Amazon S3 Standard and use an S3 Lifecycle policy to create backups.
- D. Use Amazon EFS and create an AMI whenever a database backup is required.
**Your answer:** B ✅ Correct
 
**Why:** Amazon EBS provides persistent block storage for EC2. EBS volumes keep their data when an instance is stopped and restarted, and EBS snapshots provide point-in-time backups.
 
**Why the other options are wrong:**
- **A. Instance Store:** Temporary storage; data can be lost when the instance stops, terminates, or on certain host failures.
- **C. Amazon S3:** Object storage, not a directly attached block device for a traditional database.
- **D. EFS + AMI:** EFS is shared file storage; an AMI is an instance image, not a substitute for EBS snapshots.
**Exam tip:**
- **EBS** = persistent block storage for EC2
- **EBS snapshots** = point-in-time volume backups
- **Instance Store** = temporary, host-attached storage
- **S3** = object storage
- **EFS** = managed shared file storage
---
 
#### Q53 — Cut S3 cost after 45 days, keep instant access
 
A company stores millions of documents in Amazon S3. Most are accessed frequently during the first **45 days**. After that they are rarely accessed but **must remain immediately available**. The company wants to **automatically reduce storage costs** without manually moving objects. Which solution is the best fit?
 
- A. Configure an S3 Lifecycle policy to transition the objects to S3 Standard-IA after 45 days. ✅
- B. Configure an S3 Lifecycle policy to transition the objects to S3 Glacier Deep Archive after 45 days.
- C. Create EBS snapshots of the S3 objects after 45 days.
- D. Move the objects to an EC2 instance store volume after 45 days.
**Your answer:** A ✅ Correct
 
**Why:** S3 Lifecycle policies automate object transitions between storage classes. S3 Standard-IA suits infrequently accessed data that still needs rapid retrieval. Retrieval charges and minimum storage-duration charges apply to Standard-IA.
 
**Why the other options are wrong:**
- **B. Glacier Deep Archive:** Long-term archival; retrieval typically takes hours.
- **C. EBS snapshots:** Back up EBS volumes, not S3 objects.
- **D. Instance Store:** Temporary host-attached storage, unsuitable for durable document storage.
**Exam tip:**
- **S3 Lifecycle** = automates transitions based on rules
- **S3 Standard-IA** = infrequent access, rapid retrieval
- **S3 Glacier Deep Archive** = long-term archival, slower retrieval
---
 
#### Q54 — Shared Linux file storage across AZs
 
A company needs **shared file storage** for a Linux application running on multiple EC2 instances in **different Availability Zones**. All instances must access the **same files concurrently**, and AWS should manage the underlying storage infrastructure. Which service best meets these requirements?
 
- A. Amazon EBS, with one volume attached to each EC2 instance
- B. Amazon EC2 Instance Store
- C. Amazon Elastic File System (Amazon EFS) ✅
- D. Amazon S3 Glacier Flexible Retrieval
**Your answer:** C ✅ Correct
 
**Why:** Amazon EFS is managed, scalable file storage that lets multiple EC2 instances, including those in different AZs, access the same files concurrently.
 
**Why the other options are wrong:**
- **A. EBS:** Primarily block storage; separate volumes do not automatically provide shared files.
- **B. Instance Store:** Temporary storage; data can be lost when the instance stops or terminates.
- **D. S3 Glacier Flexible Retrieval:** Archival storage, not shared file-system access.
**Exam tip:**
- **EBS** = block storage
- **EFS** = shared file storage for Linux workloads
- **S3** = object storage
- **Instance Store** = temporary, host-attached storage
- **S3 Glacier** = archival storage
---
 
#### Q55 — Windows file system with Active Directory
 
A company needs a shared file system for an application running on several Linux EC2 instances. The application requires features compatible with the **Windows file system protocol**, including **native Microsoft Windows file-system support and Microsoft Active Directory integration**. Which AWS storage service is the best fit?
 
- A. Amazon EFS
- B. Amazon EBS
- C. Amazon FSx for Windows File Server ✅
- D. Amazon S3 Standard
**Your answer:** C ✅ Correct
 
**Why:** Amazon FSx for Windows File Server provides fully managed shared file storage built on Windows Server. It supports **SMB** and integrates with **Microsoft Active Directory**.
 
**Why the other options are wrong:**
- **A. EFS:** Designed for shared file storage using NFS, commonly for Linux.
- **B. EBS:** Block storage, not a managed shared Windows file server.
- **D. S3 Standard:** Object storage, not a native Windows shared file system.
**Exam tip:**
- **EFS** = shared file storage, commonly Linux/NFS
- **FSx for Windows File Server** = Windows / SMB / Active Directory
- **EBS** = block storage
- **S3** = object storage
---
 
#### Q56 — On-premises app needing a local cache
 
A company has an on-premises application that needs **low-latency access to frequently used files**. It wants to use AWS cloud storage but must **keep a local copy of frequently accessed data** to reduce latency. Less frequently accessed files can remain in AWS. Which solution best meets these requirements?
 
- A. Amazon EBS snapshots
- B. AWS Storage Gateway using a file gateway ✅
- C. Amazon S3 Glacier Deep Archive
- D. Amazon EC2 Instance Store
**Your answer:** B ✅ Correct
 
**Why:** AWS Storage Gateway connects on-premises environments to AWS storage. A **file gateway** lets applications use standard file protocols while storing data as objects in Amazon S3, and local caching reduces latency for frequently accessed data.
 
**Why the other options are wrong:**
- **A. EBS snapshots:** Back up EBS block volumes.
- **C. Glacier Deep Archive:** Long-term archival, not frequent low-latency access.
- **D. Instance Store:** Temporary storage on an EC2 host, not a hybrid storage service.
**Exam tip:** Storage Gateway supports **hybrid storage** architectures that connect on-premises applications with AWS storage.
 
---
 
#### Q57 — Hundreds of terabytes, slow internet
 
A company needs to transfer **hundreds of terabytes** from its on-premises data center to Amazon S3. Its internet connection is too slow to finish within the required timeframe. In a traditional AWS certification scenario, the company wants **AWS to physically transport the data**. Which AWS service should it use?
 
- A. AWS DataSync
- B. AWS Database Migration Service (AWS DMS)
- C. AWS Snowball Edge ✅
- D. Amazon CloudFront
**Your answer:** C ✅ Correct
 
**Why:** AWS Snowball Edge has traditionally provided physical devices for transferring large datasets to AWS when online transfers are too slow or impractical.
 
**Why the other options are wrong:**
- **A. DataSync:** Online data transfer and synchronization.
- **B. DMS:** Database migration.
- **D. CloudFront:** Content delivery through a global network.
**Exam tip:** Huge dataset + physical device because network transfer is impractical → think **Snowball**.
 
---
 
#### Q58 — Oracle to Aurora with ongoing replication
 
A company is migrating an on-premises Oracle database to Amazon Aurora. It wants to **minimize application downtime** and keep the target database **synchronized with ongoing changes** to the source during migration. Which AWS service is the best fit?
 
- A. AWS DataSync
- B. AWS Database Migration Service (AWS DMS) ✅
- C. AWS Storage Gateway
- D. AWS Snowball Edge
**Your answer:** B ✅ Correct
 
**Why:** AWS DMS migrates databases between supported sources and targets and can replicate ongoing changes so the target stays updated, helping minimize downtime. The **AWS Schema Conversion Tool (AWS SCT)** may also be needed when converting schemas and database code between different engines.
 
**Why the other options are wrong:**
- **A. DataSync:** Transfers files and data between supported storage systems.
- **C. Storage Gateway:** Hybrid access between on-premises applications and AWS storage.
- **D. Snowball Edge:** Traditionally used for physical bulk data transfer, not ongoing database replication.
**Exam tip:**
- **DMS** = database migration and ongoing replication
- **SCT** = schema and code conversion when required
- **DataSync** = file and storage data transfer
---
 
#### Q59 — Discover existing servers before migrating
 
A company wants to move an on-premises application to AWS. Before migrating, it needs to **discover its existing servers, collect utilization data, and analyze dependencies** to plan the migration. Which AWS service is the best fit?
 
- A. AWS Migration Hub
- B. AWS Application Discovery Service ✅
- C. AWS Database Migration Service (AWS DMS)
- D. AWS DataSync
**Your answer:** not recorded in the source file
 
**Why:** AWS Application Discovery Service collects information about on-premises servers, including configuration, utilization, and application dependencies, to help plan a migration.
 
**Why the other options are wrong:**
- **A. Migration Hub:** A central place to track migration progress.
- **C. DMS:** Migrates databases.
- **D. DataSync:** Transfers and synchronizes data between supported storage systems.
**Exam tip:**
- **Discovery Service** = discover and assess the existing environment
- **Migration Hub** = track migration progress
- **DMS** = migrate databases
- **DataSync** = transfer files and storage data
---
 
#### Q60 — Online file transfer and sync to S3
 
A company stores files on an on-premises file server and wants to move them to Amazon S3 and **keep them synchronized with minimal manual effort**. It has a reliable network connection and does **not** need a physical transfer device. Which AWS service is the best choice?
 
- A. AWS DataSync ✅
- B. AWS Migration Hub
- C. AWS Storage Gateway
- D. AWS Application Discovery Service
**Your answer:** A ✅ Correct
 
**Why:** AWS DataSync automates data transfers and synchronization between on-premises storage and AWS storage services, including Amazon S3, Amazon EFS, and Amazon FSx.
 
**Why the other options are wrong:**
- **B. Migration Hub:** Tracks application migration progress centrally.
- **C. Storage Gateway:** Hybrid cloud storage integration, letting on-premises applications work with AWS storage.
- **D. Application Discovery Service:** Collects information about existing servers and dependencies to plan migrations.
**Exam tip:** Network-based transfer and sync → **DataSync**. Physical bulk transfer → **Snowball Edge**. Hybrid storage access → **Storage Gateway**.
 
---
 
#### Q61 — EC2 database: persistent block storage, snapshot backups
 
A company runs a database on an EC2 instance. It requires **persistent block storage** and wants **point-in-time backups without backing up the entire EC2 instance** each time. Which solution best meets these requirements?
 
- A. Store the database files in Amazon S3 and access them directly as a block device.
- B. Attach an Amazon EBS volume to the EC2 instance and create EBS snapshots. ✅
- C. Use an EC2 instance store volume and create EBS snapshots of it.
- D. Use Amazon EFS and create S3 Lifecycle rules.
**Your answer:** B ✅ Correct
 
**Why:** EBS provides persistent block storage for EC2, and EBS snapshots provide point-in-time backups of EBS volumes.
 
**Why the other options are wrong:**
- **A. S3:** Object storage, not directly attachable block storage.
- **C. Instance store:** Temporary storage; instance store data cannot be backed up using EBS snapshots.
- **D. EFS:** Shared file storage, not the requested block storage.
**Exam tip:** EC2 database + persistent block storage = **EBS**. Volume backup = **EBS snapshot**.
 
---
 
#### Q62 — Critical, I/O-intensive database volume
 
A company runs a database on Amazon EC2 with a **critical, I/O-intensive workload** that requires **consistently high storage performance and very high IOPS**. Which Amazon EBS volume type is the best fit?
 
- A. Throughput Optimized HDD (st1)
- B. General Purpose SSD (gp3)
- C. Cold HDD (sc1)
- D. Provisioned IOPS SSD (io2) ✅
**Your answer:** not recorded in the source file
 
**Why:** EBS **io2** is designed for I/O-intensive applications, including critical databases that need high, consistent IOPS and low latency.
 
**Why the other options are wrong:**
- **A. st1:** Optimized for high-throughput, frequently accessed workloads such as big data and log processing.
- **B. gp3:** A cost-effective general-purpose SSD suitable for many applications and databases, but io2 is the stronger fit for an explicitly demanding IOPS requirement.
- **C. sc1:** Low-cost HDD for infrequently accessed data.
**Exam tip:** High IOPS + critical database = **io2**. General-purpose SSD = gp3. High throughput = st1. Infrequent, low-cost HDD access = sc1.
 
**Key distinction:** IOPS measures *operations per second*; throughput measures *data transferred per second*.
 
---
 
#### Q63 — Temporary, recreatable data with fastest local storage
 
A company runs a temporary data-processing workload on EC2. The workload generates **intermediate data that can be recreated at any time**. The company wants the **lowest-latency local storage** and does not need the data to survive if the instance is stopped, terminated, or the hardware fails. Which storage option is the best fit?
 
- A. Amazon EBS gp3
- B. Amazon EFS
- C. EC2 Instance Store ✅
- D. Amazon S3 Standard
**Your answer:** C ✅ Correct
 
**Why:** EC2 Instance Store provides temporary, local block storage with high I/O performance. Its data is not persistent and may be lost when the instance stops, terminates, or experiences certain hardware failures.
 
**Why the other options are wrong:**
- **A. EBS gp3:** Persistent block storage.
- **B. EFS:** Shared file storage.
- **D. S3 Standard:** Durable object storage rather than local block storage.
**Exam tip:** Temporary, recreatable data + high-performance local storage = **Instance Store**. Data that must persist after a stop = **EBS**.
 
---
 
#### Q64 — Shared file system across multiple AZs
 
A company runs a Linux application on multiple EC2 instances in different Availability Zones. All instances must access the **same shared file system concurrently**, and the files must remain available independently of any single EC2 instance. Which AWS storage service best meets these requirements?
 
- A. Amazon EBS
- B. Amazon EFS ✅
- C. EC2 Instance Store
- D. Amazon S3 Glacier Deep Archive
**Your answer:** B ✅ Correct
 
**Why:** Amazon EFS provides managed, elastic file storage that multiple EC2 instances can access concurrently across Availability Zones within a Region.
 
**Why the other options are wrong:**
- **A. EBS:** Persistent block storage for EC2; not the standard choice for a shared Linux file system across multiple instances and AZs.
- **C. Instance Store:** Temporary local storage tied to an EC2 host.
- **D. S3 Glacier Deep Archive:** Long-term archival storage, not an active shared file system.
**Exam tip:** Shared Linux file system across EC2 instances and AZs = **EFS**. Persistent block storage = EBS.
 
---
 
#### Q65 — SMB + Active Directory for a Windows application
 
A company runs a Windows-based application on Amazon EC2. The application requires a **fully managed file system that supports the SMB protocol and integrates with Microsoft Active Directory**. Which AWS service should the company choose?
 
- A. Amazon EFS
- B. Amazon S3
- C. Amazon FSx for Windows File Server ✅
- D. Amazon EBS
**Your answer:** C ✅ Correct
 
**Why:** FSx for Windows File Server provides a managed Windows file system with SMB support and Microsoft Active Directory integration.
 
**Why the other options are wrong:**
- **A. EFS:** Primarily shared Linux file storage using NFS.
- **B. S3:** Object storage, not a native Windows file system.
- **D. EBS:** Block storage for EC2 instances.
**Exam tip:** Windows + SMB + Active Directory = **FSx for Windows File Server**. Shared Linux file storage = EFS.
 
---
 
#### Q66 — Cheapest long-term archive
 
A company wants to store files in Amazon S3 at the **lowest possible storage cost for long-term archival**. The files are rarely accessed, and when retrieval is required, **waiting approximately 12 hours is acceptable**. Which S3 storage class is the best choice?
 
- A. S3 Standard
- B. S3 Standard-Infrequent Access (S3 Standard-IA)
- C. S3 Glacier Flexible Retrieval
- D. S3 Glacier Deep Archive ✅
**Your answer:** D ✅ Correct
 
**Why:** S3 Glacier Deep Archive is designed for long-term retention of rarely accessed data at very low storage cost. Standard retrieval typically takes about 12 hours or longer, depending on the retrieval option.
 
**Why the other options are wrong:**
- **A. S3 Standard:** For frequently accessed data.
- **B. S3 Standard-IA:** For infrequently accessed data that needs rapid retrieval.
- **C. S3 Glacier Flexible Retrieval:** Faster retrieval options than Deep Archive, but generally more expensive for storage.
**Exam tip:** Lowest-cost long-term archive + retrieval delay of many hours = **S3 Glacier Deep Archive**.
 
---
 
#### Q67 — Recover older versions of S3 objects
 
A company stores important documents in an Amazon S3 bucket. It wants to **preserve previous versions of each object** so users can recover an older version if a file is accidentally overwritten or deleted. Which solution best meets this requirement?
 
- A. Enable S3 Versioning on the bucket. ✅
- B. Enable S3 Transfer Acceleration on the bucket.
- C. Create EBS snapshots for the bucket.
- D. Move all objects to S3 Glacier Deep Archive.
**Your answer:** A ✅ Correct
 
**Why:** S3 Versioning preserves multiple versions of objects and lets users recover previous versions after accidental overwrites or deletions.
 
**Why the other options are wrong:**
- **B. S3 Transfer Acceleration:** Speeds up uploads to S3 over long distances.
- **C. EBS snapshots:** Back up EBS volumes, not S3 buckets.
- **D. S3 Glacier Deep Archive:** Low-cost archival storage, not object version recovery by itself.
**Exam tip:** Accidental overwrite or deletion + recover older object versions = **S3 Versioning**.
 
---
 
#### Q68 — Automatically move S3 objects to cheaper storage
 
A company stores millions of objects in Amazon S3. Some are accessed frequently during the first **30 days**, then rarely. The company wants to **automatically reduce storage costs** without manually moving objects. Which solution best meets this requirement?
 
- A. Enable S3 Versioning.
- B. Configure an S3 Lifecycle rule to transition objects to a more cost-effective storage class after 30 days. ✅
- C. Create an EBS snapshot every 30 days.
- D. Use AWS DataSync to move the objects to Amazon EFS after 30 days.
**Your answer:** B ✅ Correct
 
**Why:** S3 Lifecycle rules automatically transition objects between storage classes or expire them based on age and other conditions.
 
**Why the other options are wrong:**
- **A. Versioning:** Preserves multiple versions of objects; it does not transition objects to cheaper storage.
- **C. EBS snapshots:** Back up EBS volumes, not S3 objects.
- **D. DataSync:** Transfers and synchronizes data; not the standard mechanism for transitioning S3 objects between storage classes.
**Exam tip:** Automatic S3 storage-class changes → **S3 Lifecycle rules**.
 
---
 
#### Q69 — Lower latency for global users
 
A company runs a web application on Amazon EC2 that stores images and documents in Amazon S3. Users around the world report slow downloads because the S3 bucket is in one AWS Region. The company wants to **deliver content with lower latency to users worldwide** without moving the original objects out of S3. Which AWS service should it use?
 
- A. Amazon Route 53
- B. Amazon CloudFront ✅
- C. AWS Direct Connect
- D. Amazon EBS
**Your answer:** B ✅ Correct
 
**Why:** Amazon CloudFront is a content delivery network (CDN) that caches content at globally distributed edge locations to reduce latency.
 
**Why the other options are wrong:**
- **A. Route 53:** DNS and domain-routing services.
- **C. Direct Connect:** A dedicated network connection to AWS.
- **D. EBS:** Block storage for EC2 instances.
**Exam tip:** Global content delivery + lower latency + edge caching = **Amazon CloudFront**.
 
---
 
#### Q70 — Survive an AZ failure and distribute traffic
 
A company hosts a business-critical application on EC2. It wants the application to remain available even if **one Availability Zone becomes unavailable**, and wants incoming traffic distributed across healthy instances automatically. Which solution best meets both requirements?
 
- A. Launch one large EC2 instance in a single Availability Zone.
- B. Deploy EC2 instances across multiple Availability Zones and use an Elastic Load Balancer. ✅
- C. Store application files in Amazon S3 and use AWS CloudTrail.
- D. Deploy EC2 instances in multiple Regions but send all traffic to one instance without load balancing.
**Your answer:** B ✅ Correct
 
**Why:** Deploying instances across multiple AZs improves availability if one AZ fails, and ELB distributes incoming traffic across healthy targets.
 
**Why the other options are wrong:**
- **A:** A single instance in one AZ is a single point of failure.
- **C:** S3 provides object storage and CloudTrail records API activity; neither provides traffic distribution or AZ resilience.
- **D:** Multiple Regions can support broader resilience, but sending all traffic to one instance without load balancing neither distributes traffic nor removes the single point of failure.
**Exam tip:** AZ failure + traffic distribution = **multiple AZs + ELB**.
 
---
 
#### Q71 — Auto add/remove EC2 instances for demand
 
A company runs an application on Amazon EC2 and wants to **automatically add instances when demand increases and remove them when demand decreases**, maintaining performance while avoiding unnecessary compute costs. Which AWS service should it use?
 
- A. Elastic Load Balancing (ELB)
- B. Amazon CloudWatch only
- C. Amazon EC2 Auto Scaling ✅
- D. AWS CloudTrail
**Your answer:** C ✅ Correct
 
**Why:** EC2 Auto Scaling adjusts the number of EC2 instances according to configured scaling policies and conditions, helping maintain performance and control costs.
 
**Why the other options are wrong:**
- **A. ELB:** Distributes incoming traffic; it does not independently manage the number of instances.
- **B. CloudWatch:** Monitors metrics and provides alarms; monitoring alone does not add or remove instances.
- **D. CloudTrail:** Records AWS account activity and API calls.
**Exam tip:** Add/remove EC2 instances = **EC2 Auto Scaling**. Distribute requests = **ELB**. Monitor metrics = **CloudWatch**.
 
---
 
#### Q72 — Run code when an object is uploaded to S3
 
A company wants to run a short piece of code whenever a **new object is uploaded to an Amazon S3 bucket**. The code usually finishes within seconds. The company does not want to provision or manage servers and wants the code to run in response to the upload event. Which AWS service is the best choice?
 
- A. Amazon EC2
- B. AWS Lambda ✅
- C. Amazon ECS on EC2
- D. AWS Elastic Beanstalk
**Your answer:** B ✅ Correct
 
**Why:** AWS Lambda runs code in response to events without requiring you to provision or manage servers. S3 can trigger a Lambda function when a new object is uploaded.
 
**Why the other options are wrong:**
- **A. EC2:** Requires managing virtual server resources and the OS.
- **C. ECS on EC2:** Runs containers but requires managing EC2 capacity.
- **D. Elastic Beanstalk:** Simplifies application deployment but is not the best fit for this event-triggered function.
**Exam tip:** Short, event-driven code + no server management = **AWS Lambda**.
 
---
 
#### Q73 — Containers, AWS manages the servers
 
A company packages its application into **Docker containers**. It wants AWS to manage the underlying servers so its team can focus on deploying and running containers **without provisioning or managing EC2 instances**. Which solution best meets this requirement?
 
- A. Amazon ECS using AWS Fargate ✅
- B. Amazon ECS using the EC2 launch type
- C. Amazon EC2 Auto Scaling without a container orchestration service
- D. AWS Lambda without using container images
**Your answer:** A ✅ Correct
 
**Why:** Amazon ECS orchestrates containers, while AWS Fargate runs them without requiring you to provision or manage the underlying EC2 instances.
 
**Why the other options are wrong:**
- **B. ECS on EC2:** You manage the EC2 instances that provide container capacity.
- **C. EC2 Auto Scaling:** Manages instance capacity but provides no container orchestration by itself.
- **D. Lambda:** Suited to event-driven functions rather than serving as the general-purpose ECS container platform described here.
**Exam tip:** Containers without managing servers = **ECS with Fargate**. Containers with control over the EC2 instances = **ECS on EC2 (EC2 launch type)**.
 
---
 
#### Q74 — Predictable 3-year workload with broad flexibility ❌
 
A company runs a predictable production workload on EC2. It expects to use the compute capacity **continuously for the next three years** and wants to **reduce costs compared with On-Demand pricing**. It also wants **flexibility to change instance families, instance sizes, operating systems, and Regions** while benefiting from its commitment. Which purchasing option is the best fit?
 
- A. Spot Instances
- B. On-Demand Instances
- C. EC2 Reserved Instances only
- D. Compute Savings Plans ✅
**Your answer:** C ❌ Incorrect — the correct answer is **D**
 
**Why:** Compute Savings Plans give discounted compute pricing in exchange for a 1- or 3-year commitment to a consistent amount of usage (USD per hour). They are flexible across EC2 instance families, sizes, Regions, and operating systems, and can also apply to Lambda and Fargate usage.
 
**Why the other options are wrong:**
- **A. Spot:** Uses spare EC2 capacity at a discount but can be interrupted.
- **B. On-Demand:** No long-term commitment, but generally costs more than commitment-based pricing for steady usage.
- **C. Reserved Instances:** Can reduce EC2 costs but do not offer the same broad flexibility as Compute Savings Plans.
**Exam tip:** Broad compute flexibility + long-term commitment = **Compute Savings Plans**. A committed EC2 discount with more limited flexibility = **Reserved Instances**.
 
> 💡 **Remember:** The giveaway words are *change instance families, Regions, operating systems* → flexibility → **Savings Plans**, not Reserved Instances.
 
---
 
#### Q75 — Interruptible batch job, lowest cost
 
A company has a **batch-processing workload that can tolerate interruptions**. It runs only when spare compute capacity is available, and the company wants to **minimize EC2 costs as much as possible**. If AWS needs the capacity back, the workload can pause or restart later. Which EC2 purchasing option is the best fit?
 
- A. On-Demand Instances
- B. Spot Instances ✅
- C. Dedicated Hosts
- D. EC2 Reserved Instances
**Your answer:** B ✅ Correct
 
**Why:** Spot Instances use spare EC2 capacity at discounted prices. AWS can reclaim the capacity, so they suit fault-tolerant workloads that tolerate interruptions.
 
**Why the other options are wrong:**
- **A. On-Demand:** Flexible with no long-term commitment, but generally more expensive than Spot.
- **C. Dedicated Hosts:** A physical server dedicated to one customer, often for licensing or compliance needs.
- **D. Reserved Instances:** Discounts for committed usage rather than spare-capacity pricing.
**Exam tip:** Interruptible workload + lowest possible EC2 cost = **Spot Instances**.
 
---
 
#### Q76 — Unpredictable, short-term, no interruptions
 
A company needs to run a **short-term** application on EC2 for an **unpredictable period**. It does not want a long-term commitment, and the application **must not be interrupted because AWS reclaims spare capacity**. Which EC2 purchasing option is the best fit?
 
- A. Spot Instances
- B. EC2 Reserved Instances
- C. On-Demand Instances ✅
- D. Dedicated Hosts
**Your answer:** C ✅ Correct
 
**Why:** On-Demand Instances provide EC2 capacity without a long-term commitment. They suit unpredictable workloads that cannot tolerate Spot capacity reclamation.
 
**Why the other options are wrong:**
- **A. Spot:** Can be interrupted when AWS reclaims the capacity.
- **B. Reserved Instances:** Intended for workloads where a 1- or 3-year commitment is appropriate.
- **D. Dedicated Hosts:** Dedicated physical servers, often for licensing or compliance requirements.
**Exam tip:** Unpredictable usage + no long-term commitment + avoid Spot interruptions = **On-Demand Instances**.
 
---
#### Q77 — CLF-C02 Exam Level

A company operates a database on Amazon EC2. Its software license is tied to **physical server sockets and cores**, and the company needs visibility into the underlying physical server to meet its licensing requirements.

Which EC2 purchasing option best meets this requirement?

- A. Spot Instances
- B. Dedicated Hosts ✅
- C. On-Demand Instances on shared hardware
- D. Compute Savings Plans

**Your answer:** B. Dedicated Hosts

**Correct answer:** B. Dedicated Hosts

**Explanation:** EC2 Dedicated Hosts provide physical servers dedicated to one customer and visibility into the underlying host resources. This can help meet certain server-bound licensing and compliance requirements.

**Why the other options are incorrect:**
- **A. Spot:** Uses spare EC2 capacity and can be interrupted.
- **C. Shared hardware:** Does not provide a dedicated physical server.
- **D. Compute Savings Plans:** Provides usage-based compute discounts, not dedicated hardware.

**Exam tip:** Socket/core-based licensing + physical server visibility = Dedicated Hosts.

---

#### Q78 — CLF-C02 Exam Level

A company operates multiple AWS accounts for different departments. The finance team wants to **combine billing across all accounts** so the company can take advantage of volume discounts and manage costs centrally.

Which AWS service or feature best meets this requirement?

- A. AWS Cost Explorer
- B. AWS Organizations with consolidated billing ✅
- C. AWS Budgets
- D. AWS Control Tower

**Your answer:** B. AWS Organizations with consolidated billing

**Correct answer:** B. AWS Organizations with consolidated billing

**Explanation:** AWS Organizations lets a company manage multiple AWS accounts centrally. With **consolidated billing**, the company receives one combined bill for its accounts and can potentially benefit from aggregated usage and volume pricing benefits where eligible.

**Why the other options are incorrect:**
- **A. AWS Cost Explorer:** Analyzes spending and usage; it does not consolidate account billing by itself.
- **C. AWS Budgets:** Tracks spending against budgets and can send alerts when thresholds are reached.
- **D. AWS Control Tower:** Helps set up and govern a multi-account AWS environment using recommended controls and a landing zone.

**Exam tip:**
- Multiple accounts + one bill → **AWS Organizations with consolidated billing**
- Analyze spending → **Cost Explorer**
- Spending alerts → **AWS Budgets**
- Set up and govern a multi-account environment → **Control Tower**

---

#### Q79 — CLF-C02 Exam Level

A company wants to ensure that its developers can create and manage AWS resources, but **only within a specific set of permissions**. Developers should not be able to perform administrative actions such as changing security policies or creating new IAM users.

Which approach follows the AWS security best practice?

- A. Give all developers administrator permissions so they can work without delays.
- B. Share one IAM administrator user's credentials among all developers.
- C. Apply the principle of least privilege by granting only the permissions required for each developer's job. ✅
- D. Store the AWS root user's credentials in a shared company password manager and let developers use them when necessary.

**Your answer:** C. Apply the principle of least privilege by granting only the permissions required for each developer's job.

**Correct answer:** C. Apply the principle of least privilege.

**Explanation:** The **principle of least privilege** means granting a user or service only the permissions necessary to perform its required tasks.

For example, a developer who needs to read objects from a particular S3 bucket should not automatically receive permission to delete buckets or create IAM users.

AWS IAM policies control which actions are allowed or denied on which resources.

**Why the other options are incorrect:**
- **A:** Administrator access grants excessive permissions and increases security risk.
- **B:** Shared credentials weaken accountability and make it difficult to identify who performed an action.
- **D:** The root user has extensive privileges and should not be used for routine development work.

**Exam tip:** When you see minimum permissions, only required access, or reduce unnecessary access, think principle of least privilege.

Remember: use individual identities, grant appropriate permissions, and protect the root user.

---

#### Q80 — CLF-C02 Exam Level

A company requires an additional layer of protection for its AWS accounts. Even if an attacker discovers an employee's password, the attacker should not be able to sign in without a second verification method.

Which AWS security measure best meets this requirement?

- A. Enable multi-factor authentication (MFA). ✅
- B. Use AWS CloudTrail to record login activity.
- C. Use AWS Shield to protect against distributed denial-of-service attacks.
- D. Use Amazon GuardDuty to detect potentially malicious activity.

**Your answer:** A. Enable multi-factor authentication (MFA).

**Correct answer:** A. Enable multi-factor authentication (MFA).

**Explanation:** MFA adds an extra authentication factor beyond a password, such as a code or approval from an authenticator device. This makes account access harder for an attacker who has only obtained the password.

Enable MFA for the root user and IAM users where applicable, and follow your organization's identity-management practices.

**Why the other options are incorrect:**
- **B. CloudTrail:** Records account activity and API events for auditing and investigation.
- **C. AWS Shield:** Helps protect applications against distributed denial-of-service (DDoS) attacks.
- **D. Amazon GuardDuty:** Detects potential threats by analyzing relevant activity and data sources.

**Exam tip:**
- Extra sign-in verification → **MFA**
- Record who did what → **CloudTrail**
- Detect potential threats → **GuardDuty**
- DDoS protection → **AWS Shield**

---

#### Q81 — CLF-C02 Exam Level

A company stores database passwords and API keys used by its applications. The security team wants to store these secrets securely and rotate database credentials automatically, rather than hard-coding them into application code.

Which AWS service best meets this requirement?

- A. AWS Key Management Service (AWS KMS)
- B. AWS Secrets Manager ✅
- C. AWS CloudTrail
- D. AWS Certificate Manager (ACM)

**Your answer:** B. AWS Secrets Manager

**Correct answer:** B. AWS Secrets Manager

**Explanation:** AWS Secrets Manager securely stores, retrieves, and manages secrets such as database credentials, API keys, and passwords. It supports automatic rotation for supported integrations and can help applications retrieve secrets at runtime instead of hard-coding them.

**Why the other options are incorrect:**
- **A. AWS KMS:** Creates and manages cryptographic keys used to encrypt and decrypt data. It is not primarily a database-password management service.
- **C. AWS CloudTrail:** Records AWS account activity and API events for auditing.
- **D. AWS Certificate Manager (ACM):** Provisions and manages supported SSL/TLS certificates used to secure network communications.

**Exam tip:**
- Store and rotate passwords/API keys → **Secrets Manager**
- Manage encryption keys → **KMS**
- Audit AWS API activity → **CloudTrail**
- Manage SSL/TLS certificates → **ACM**

---

#### Q82 — CLF-C02 Exam Level

A company wants to encrypt sensitive data stored in AWS services. Its security team also needs to centrally control encryption keys and manage permissions for their use.

Which AWS service is designed for this purpose?

- A. AWS Secrets Manager
- B. AWS Key Management Service (AWS KMS) ✅
- C. Amazon GuardDuty
- D. AWS IAM Identity Center

**Your answer:** B. AWS Key Management Service (AWS KMS)

**Correct answer:** B. AWS Key Management Service (AWS KMS)

**Explanation:** AWS KMS allows customers to create and manage cryptographic keys and control who can use them. It integrates with many AWS services to support encryption at rest and other cryptographic operations.

For example, an organization can configure supported Amazon S3 or Amazon EBS resources to use KMS keys for encryption.

**Why the other options are incorrect:**
- **A. Secrets Manager:** Stores and manages sensitive values such as database credentials and API keys.
- **C. GuardDuty:** Detects potential security threats using relevant activity and data sources.
- **D. IAM Identity Center:** Manages workforce access and single sign-on across AWS accounts and applications.

**Exam tip:** **KMS = encryption keys. Secrets Manager = passwords and secrets.**

---

#### Q83 — CLF-C02 Exam Level

A company hosts a web application on AWS. It wants to protect the application from common web exploits, such as SQL injection and cross-site scripting (XSS).

Which AWS service is the best fit?

- A. AWS Shield
- B. AWS WAF (Web Application Firewall) ✅
- C. Amazon GuardDuty
- D. AWS CloudTrail

**Correct answer:** B. AWS WAF (Web Application Firewall)

**Explanation:** AWS WAF helps protect web applications by filtering HTTP(S) requests using configurable rules. It can help block common web exploits, including SQL injection and cross-site scripting.

**Why the other options are incorrect:**
- **A. AWS Shield:** Protects applications against distributed denial-of-service (DDoS) attacks.
- **C. Amazon GuardDuty:** Detects potential threats, such as suspicious activity or compromised credentials. It is not the primary service for filtering web requests against SQL injection and XSS.
- **D. AWS CloudTrail:** Records AWS account activity and API events for auditing.

**Exam tip:**
- SQL injection or XSS → **AWS WAF**
- DDoS attacks → **AWS Shield**
- Threat detection → **Amazon GuardDuty**
- Audit API activity → **AWS CloudTrail**

---

#### Q84 — CLF-C02 Exam Level

A company needs to assess its AWS environment for security misconfigurations and identify resources that may be unintentionally accessible from outside the organization. The company wants a service that analyzes resource policies and identifies unintended external access.

Which AWS service best meets this requirement?

- A. Amazon GuardDuty
- B. AWS IAM Access Analyzer ✅
- C. AWS WAF
- D. AWS Secrets Manager

**Correct answer:** B. AWS IAM Access Analyzer

**Explanation:** IAM Access Analyzer helps identify resources shared with external entities and analyzes access policies to help identify unintended access. Its findings help administrators review and correct access configurations.

**Why the other options are incorrect:**
- **A. GuardDuty:** Detects potential security threats from relevant activity and data sources.
- **C. AWS WAF:** Filters web requests to protect applications from common web exploits.
- **D. Secrets Manager:** Securely stores and manages secrets such as passwords and API keys.

**Exam tip:**
- Unexpected external resource access → **IAM Access Analyzer**
- Suspicious activity or potential threats → **GuardDuty**
- Malicious web requests → **AWS WAF**
- Passwords and API keys → **Secrets Manager**

---

#### Q85 — CLF-C02 Exam Level

A company must maintain a record of AWS API calls so its security team can investigate **who made a change, what action they performed, and when it happened**.

Which AWS service should the company use?

- A. Amazon CloudWatch
- B. AWS CloudTrail ✅
- C. AWS Trusted Advisor
- D. AWS Config

**Your answer:** B. AWS CloudTrail

**Correct answer:** B. AWS CloudTrail

**Explanation:** AWS CloudTrail records AWS account activity, including API actions performed by users, roles, and services. Its event records can help answer who performed an action, what happened, when it occurred, and other relevant details.

For example, if an S3 bucket policy was changed, CloudTrail can help investigators identify the API event associated with that change.

**Why the other options are incorrect:**
- **A. CloudWatch:** Monitors metrics, logs, alarms, and operational performance.
- **C. Trusted Advisor:** Provides recommendations and checks across areas such as cost optimization, security, and performance.
- **D. AWS Config:** Records resource configurations and evaluates them against desired rules.

**Exam tip:**
- **Who did what and when?** → CloudTrail
- **How is the application performing?** → CloudWatch
- **What changed in the resource configuration?** → AWS Config
- **What can be improved?** → Trusted Advisor

---

#### Q86 — CLF-C02 Exam Level

A company wants to continuously monitor whether its AWS resources comply with internal configuration rules. For example, it wants to identify S3 buckets that do not meet its required configuration and review the history of configuration changes.

Which AWS service best meets this requirement?

- A. AWS CloudTrail
- B. Amazon CloudWatch
- C. AWS Config ✅
- D. Amazon GuardDuty

**Your answer:** C. AWS Config

**Correct answer:** C. AWS Config

**Explanation:** AWS Config records and evaluates the configurations of supported AWS resources. It can help determine whether resources comply with specified rules and provide a history of configuration changes.

For example, a company can use AWS Config rules to evaluate whether S3 buckets meet its security requirements.

**Why the other options are incorrect:**
- **A. CloudTrail:** Records AWS API activity, including actions that change resources.
- **B. CloudWatch:** Monitors metrics, logs, alarms, and operational performance.
- **D. GuardDuty:** Detects potential security threats and suspicious activity.

**Exam tip:**
- Resource configuration and compliance → **AWS Config**
- API activity and auditing → **CloudTrail**
- Metrics, logs, and alarms → **CloudWatch**
- Threat detection → **GuardDuty**

---

#### Q87 — CLF-C02 Exam Level

A company runs an application on Amazon EC2. The operations team wants to monitor CPU utilization and automatically receive an alert when CPU utilization exceeds 80% for a sustained period.

Which solution best meets this requirement?

- A. AWS CloudTrail event history
- B. Amazon CloudWatch alarm ✅
- C. AWS Config rule
- D. AWS IAM Access Analyzer

**Your answer:** B. Amazon CloudWatch alarm

**Correct answer:** B. Amazon CloudWatch alarm

**Explanation:** Amazon CloudWatch collects monitoring data, including EC2 CPU utilization metrics. A CloudWatch alarm evaluates a metric against a defined threshold over a specified period and can trigger an action, such as sending an Amazon SNS notification.

For this scenario, configure an alarm for CPU utilization above 80% for the required duration.

**Why the other options are incorrect:**
- **A. CloudTrail:** Records API activity, not CPU threshold monitoring.
- **C. AWS Config:** Evaluates resource configuration compliance, not CPU utilization thresholds.
- **D. IAM Access Analyzer:** Helps identify unintended resource access.

**Exam tip:** When a question mentions a metric, threshold, sustained period, or automatic alert, think **CloudWatch alarm**.

---

#### Q88 — CLF-C02 Exam Level

A company wants to deploy a web application without manually provisioning servers, configuring load balancing, or managing much of the underlying infrastructure. Developers want to upload their application code, while AWS handles much of the environment setup and management.

Which AWS service is the best fit?

- A. Amazon EC2
- B. AWS Elastic Beanstalk ✅
- C. AWS CloudFormation
- D. AWS Organizations

**Your answer:** B. AWS Elastic Beanstalk

**Correct answer:** B. AWS Elastic Beanstalk

**Explanation:** AWS Elastic Beanstalk is an application deployment and management service. Developers provide their application code, and Elastic Beanstalk provisions and manages supporting resources such as EC2 instances, load balancing, and scaling, depending on the chosen environment configuration.

It reduces infrastructure-management effort, although users remain responsible for their application and relevant configurations.

**Why the other options are incorrect:**
- **A. Amazon EC2:** Provides virtual servers but requires more direct infrastructure management.
- **C. AWS CloudFormation:** Provisions infrastructure using templates; it is not specifically a managed application deployment platform.
- **D. AWS Organizations:** Centrally manages multiple AWS accounts and supports consolidated billing.

**Exam tip:**
- Upload application code and simplify deployment → **Elastic Beanstalk**
- Control virtual servers directly → **EC2**
- Define infrastructure in templates → **CloudFormation**

---

#### Q89 — CLF-C02 Exam Level

A company needs to run a small website and a simple application in AWS. It has limited cloud expertise and wants a straightforward service with simplified server, networking, and deployment management rather than configuring many individual AWS services.

Which AWS service is the best fit?

- A. Amazon Lightsail ✅
- B. Amazon EC2 with a custom VPC
- C. AWS Outposts
- D. AWS Direct Connect

**Your answer:** A. Amazon Lightsail

**Correct answer:** A. Amazon Lightsail

**Explanation:** Amazon Lightsail provides an easy-to-use platform for launching websites and simple applications using bundled resources and predictable pricing.

**Why the other options are incorrect:**
- **B. EC2 with a custom VPC:** Offers greater control but requires more infrastructure configuration.
- **C. AWS Outposts:** Brings AWS infrastructure to an on-premises facility.
- **D. AWS Direct Connect:** Provides dedicated network connectivity to AWS.

**Exam tip:** Simple website hosting with minimal infrastructure complexity → **Amazon Lightsail**.

---

#### Q90 — CLF-C02 Exam Level

A company wants to connect its on-premises data center to its AWS VPC using a dedicated network connection. It wants more consistent network performance than a connection that relies entirely on the public internet.

Which AWS service should it choose?

- A. AWS VPN
- B. AWS Direct Connect ✅
- C. Amazon CloudFront
- D. AWS Outposts

**Your answer:** B. AWS Direct Connect

**Correct answer:** B. AWS Direct Connect

**Explanation:** AWS Direct Connect establishes a dedicated network connection between a customer's network and AWS. It can provide more consistent network performance than relying solely on internet connectivity.

A Direct Connect connection is not automatically encrypted; additional encryption mechanisms may be needed when encryption in transit is required.

**Why the other options are incorrect:**
- **A. AWS VPN:** Creates an encrypted connection over an existing network, commonly the internet. It does not itself provide a dedicated physical connection.
- **C. Amazon CloudFront:** A content delivery network that caches and delivers content closer to users.
- **D. AWS Outposts:** Deploys supported AWS infrastructure and services at a customer's on-premises location.

**Exam tip:**
- Dedicated connection to AWS → **Direct Connect**
- Encrypted tunnel over the internet → **VPN**
- Low-latency content delivery → **CloudFront**
- AWS infrastructure on-premises → **Outposts**

---

#### Q91 — CLF-C02 Exam Level

A company hosts a video streaming website in AWS. Viewers around the world experience slow loading because the videos are served from a single origin location. The company wants to cache content at locations closer to viewers to reduce latency.

Which AWS service is the best fit?

- A. AWS Direct Connect
- B. Amazon CloudFront ✅
- C. Amazon Route 53
- D. AWS Transit Gateway

**Your answer:** B. Amazon CloudFront

**Correct answer:** B. Amazon CloudFront

**Explanation:** Amazon CloudFront is AWS's content delivery network. It caches and delivers supported content through geographically distributed edge locations, helping reduce latency and improve delivery performance.

For example, viewers may receive cached video content from an edge location closer to them instead of retrieving every request from the origin server.

**Why the other options are incorrect:**
- **A. Direct Connect:** Provides dedicated network connectivity between a customer network and AWS.
- **C. Route 53:** Provides DNS, domain registration, and routing capabilities.
- **D. Transit Gateway:** Connects multiple VPCs and on-premises networks through a central network hub.

**Exam tip:**
- Cache content near global users → **CloudFront**
- Translate domain names into routing information → **Route 53**
- Connect many VPCs and networks → **Transit Gateway**
- Dedicated data-center connectivity → **Direct Connect**

---

#### Q92 — CLF-C02 Exam Level

A company operates several VPCs across multiple AWS accounts and also has an on-premises network. It wants to connect these networks through a central hub instead of creating and maintaining separate connections between every network.

Which AWS service best meets this requirement?

- A. Amazon CloudFront
- B. Amazon Route 53
- C. AWS Transit Gateway ✅
- D. AWS Direct Connect

**Your answer:** C. AWS Transit Gateway

**Correct answer:** C. AWS Transit Gateway

**Explanation:** AWS Transit Gateway acts as a central hub for connecting multiple VPCs and on-premises networks. It simplifies network architecture by allowing connected networks to communicate through a central connection point rather than requiring many separate point-to-point connections.

**Why the other options are incorrect:**
- **A. CloudFront:** Delivers and caches content closer to end users.
- **B. Route 53:** Provides DNS and routing capabilities for directing traffic.
- **D. Direct Connect:** Establishes dedicated network connectivity between a customer's network and AWS; it is not the central hub that connects many VPCs.

**Exam tip:**
- Central hub for multiple VPCs and networks → **Transit Gateway**
- Dedicated connection from on-premises to AWS → **Direct Connect**
- Global content delivery → **CloudFront**
- DNS and domain routing → **Route 53**

---

#### Q93 — CLF-C02 Exam Level

A company needs to connect its on-premises data center securely to an Amazon VPC. It wants to use an encrypted tunnel over the public internet and does not require a dedicated physical connection.

Which AWS solution is the best fit?

- A. AWS Site-to-Site VPN ✅
- B. AWS Direct Connect
- C. Amazon CloudFront
- D. AWS Transit Gateway

**Your answer:** A. AWS Site-to-Site VPN

**Correct answer:** A. AWS Site-to-Site VPN

**Explanation:** AWS Site-to-Site VPN creates encrypted VPN tunnels between an on-premises network and an AWS VPC through the internet. It is suitable when secure connectivity is required without establishing a dedicated physical connection.

**Why the other options are incorrect:**
- **B. Direct Connect:** Provides dedicated network connectivity but does not automatically encrypt traffic.
- **C. CloudFront:** Caches and delivers content globally.
- **D. Transit Gateway:** Centrally connects multiple VPCs and on-premises networks; it can work alongside VPN connections but is not itself the VPN tunnel.

**Exam tip:**
- Encrypted tunnel over the internet → **Site-to-Site VPN**
- Dedicated network connection → **Direct Connect**
- Central network hub → **Transit Gateway**

---

#### Q94 — CLF-C02 Exam Level

A company is designing a VPC security architecture. It wants to control inbound and outbound traffic for individual EC2 instances using stateful rules. Return traffic for an allowed connection should be permitted automatically.

Which AWS feature should the company use?

- A. Network ACLs (NACLs)
- B. Security groups ✅
- C. AWS WAF
- D. AWS Shield

**Correct answer:** B. Security groups

**Explanation:** Security groups are stateful virtual firewalls associated with resources such as EC2 instances. Return traffic for an allowed connection is automatically permitted.

**Why the other options are incorrect:**
- **A. NACLs:** Stateless traffic filters applied at the subnet level.
- **C. AWS WAF:** Filters web requests against configured rules.
- **D. AWS Shield:** Protects applications against DDoS attacks.

**Exam tip:** **Security group = stateful, resource level. NACL = stateless, subnet level.**

---

#### Q95 — CLF-C02 Exam Level

A company wants to block traffic from a specific IP address range at the subnet boundary. Its security team needs rules that explicitly control both inbound and outbound traffic, including response traffic.

Which VPC feature is the best fit?

- A. Security groups
- B. Network ACLs (NACLs) ✅
- C. AWS IAM policies
- D. AWS WAF

**Your answer:** B. Network ACLs (NACLs)

**Correct answer:** B. Network ACLs (NACLs)

**Explanation:** Network ACLs are stateless traffic filters associated with subnets. They can allow or deny traffic based on rules, including IP address ranges and ports.

Because NACLs are stateless, inbound and outbound traffic must be evaluated separately. For example, permitting an inbound request does not automatically permit its response traffic.

**Why the other options are incorrect:**
- **A. Security groups:** Stateful firewalls associated with resources such as EC2 instances.
- **C. IAM policies:** Control permissions for AWS identities and resources, not network packet filtering at a subnet boundary.
- **D. AWS WAF:** Filters HTTP(S) requests to protect web applications.

**Exam tip:**
- Subnet boundary + explicit allow/deny rules → **NACL**
- Individual resource + stateful rules → **Security group**

---

#### Q96 — CLF-C02 Exam Level

A company needs to distribute incoming application traffic across EC2 instances in multiple Availability Zones. It also wants unhealthy instances to stop receiving requests.

Which AWS service best meets this requirement?

- A. Amazon Route 53
- B. Elastic Load Balancing (ELB) ✅
- C. AWS Transit Gateway
- D. Amazon CloudFront

**Your answer:** B. Elastic Load Balancing (ELB)

**Correct answer:** B. Elastic Load Balancing (ELB)

**Explanation:** Elastic Load Balancing distributes incoming application traffic across registered targets, which may include EC2 instances in multiple Availability Zones. Health checks help determine whether targets are healthy so the load balancer can avoid routing requests to unhealthy targets.

**Why the other options are incorrect:**
- **A. Route 53:** Provides DNS and routing capabilities, including DNS-based routing policies and health-check integration.
- **C. Transit Gateway:** Connects VPCs and on-premises networks through a central hub.
- **D. CloudFront:** Caches and delivers content through a global network of edge locations.

**Exam tip:**
- Distribute application traffic across targets → **ELB**
- Add or remove EC2 instances as demand changes → **EC2 Auto Scaling**
- Resolve domain names and route DNS queries → **Route 53**

---

#### Q97 — CLF-C02 Exam Level

A company wants to make its application highly available across two Availability Zones. It wants the application to automatically launch replacement EC2 instances when existing instances fail and add more instances when demand increases.

Which solution best meets both requirements?

- A. Use an Application Load Balancer alone.
- B. Use EC2 Auto Scaling across multiple Availability Zones, together with an Elastic Load Balancer. ✅
- C. Use Amazon CloudFront with a single EC2 instance.
- D. Use AWS CloudTrail with EC2 On-Demand Instances.

**Your answer:** B. Use EC2 Auto Scaling across multiple Availability Zones, together with an Elastic Load Balancer.

**Correct answer:** B. Use EC2 Auto Scaling across multiple Availability Zones, together with an Elastic Load Balancer.

**Explanation:** The two services have different roles:

- **EC2 Auto Scaling:** Maintains the desired number of instances and can replace unhealthy instances or scale capacity according to configured policies.
- **Elastic Load Balancing:** Distributes incoming traffic across healthy targets.

Deploying across multiple Availability Zones helps reduce dependence on a single location within a Region.

**Why the other options are incorrect:**
- **A. Application Load Balancer alone:** Distributes traffic but does not independently provide EC2 capacity scaling and replacement.
- **C. CloudFront with one EC2 instance:** CloudFront helps deliver content but does not make a single EC2 instance highly available.
- **D. CloudTrail with On-Demand Instances:** CloudTrail records account activity; On-Demand is a pricing model, not an availability strategy.

**Exam tip:** **Auto Scaling = manage capacity. ELB = distribute traffic. Multiple AZs = improve availability.**

---

#### Q98 — CLF-C02 Exam Level

A company needs to run a scheduled data-processing task every night. The task runs for approximately 10 minutes, and the company does not want to provision or manage servers. It wants to pay for compute time used rather than maintain an always-running server.

Which AWS service is the best fit?

- A. Amazon EC2
- B. AWS Lambda ✅
- C. Amazon Lightsail
- D. AWS Outposts

**Your answer:** B. AWS Lambda

**Correct answer:** B. AWS Lambda

**Explanation:** AWS Lambda runs code without requiring you to provision or manage servers. It supports event-driven execution, including scheduled invocations, and charges are based on usage rather than maintaining an always-running server.

Lambda has execution limits and is best suited to workloads that fit its supported execution model.

**Why the other options are incorrect:**
- **A. EC2:** Provides virtual servers that require more direct management.
- **C. Lightsail:** Simplifies hosting for websites and applications, but isn't the best fit for a short scheduled function.
- **D. Outposts:** Provides supported AWS infrastructure on-premises.

**Exam tip:** Short event-driven or scheduled code without server management → **AWS Lambda**.

---

#### Q99 — CLF-C02 Exam Level

A company wants to build a decoupled application. One component should send messages to a queue so another component can process them independently, even if the consumer is temporarily unavailable.

Which AWS service should the company use?

- A. Amazon Simple Notification Service (Amazon SNS)
- B. Amazon Simple Queue Service (Amazon SQS) ✅
- C. Amazon EventBridge
- D. AWS Step Functions

**Your answer:** B. Amazon Simple Queue Service (Amazon SQS)

**Correct answer:** B. Amazon Simple Queue Service (Amazon SQS)

**Explanation:** Amazon SQS is a managed message-queuing service. It allows producers to send messages to a queue while consumers retrieve and process those messages independently. Messages can remain queued while consumers are temporarily unavailable, subject to the queue's retention and configuration settings.

This decoupling helps applications handle variable workloads and reduces direct dependencies between components.

**Why the other options are incorrect:**
- **A. SNS:** Primarily distributes notifications and messages to subscribers.
- **C. EventBridge:** Routes events to targets based on event rules.
- **D. Step Functions:** Coordinates multi-step workflows.

**Exam tip:** **Queue = SQS. Notifications to subscribers = SNS. Event rules = EventBridge. Workflow steps = Step Functions.**

---

#### Q100 — CLF-C02 Exam Level

A company wants to review its AWS spending over the past six months, identify which services are increasing its costs, and forecast its spending for the next three months.

The company does not need to create a fixed spending limit or receive budget-threshold alerts.

Which AWS service best meets these requirements?

- A. AWS Budgets
- B. AWS Cost Explorer ✅
- C. AWS Pricing Calculator
- D. AWS Trusted Advisor

**Your answer:** B. AWS Cost Explorer

**Correct answer:** B. AWS Cost Explorer

**Explanation:** AWS Cost Explorer visualizes historical AWS costs and usage, helps identify spending trends, and provides cost forecasts.

**Why the other options are incorrect:**
- **A. AWS Budgets:** Sets custom cost or usage budgets and can send alerts when thresholds are reached or forecast to be exceeded.
- **C. AWS Pricing Calculator:** Estimates the cost of planned AWS services before deploying them.
- **D. AWS Trusted Advisor:** Provides recommendations across areas such as cost optimization, security, performance, fault tolerance, and service limits.

**Exam tip:**
- Analyze past spending and forecast future costs → **Cost Explorer**
- Set spending thresholds and receive alerts → **AWS Budgets**
- Estimate the cost of a planned architecture → **AWS Pricing Calculator**
- Receive AWS best-practice recommendations → **Trusted Advisor**

---

#### Q101 — CLF-C02 Exam Level

A company wants to receive an alert when its monthly AWS spending is forecast to exceed $1,000, even if its actual spending has not yet reached that amount.

Which AWS service should the company use?

- A. AWS Cost Explorer
- B. AWS Budgets ✅
- C. AWS Pricing Calculator
- D. AWS CloudTrail

**Your answer:** B. AWS Budgets

**Correct answer:** B. AWS Budgets

**Explanation:** AWS Budgets allows users to create cost budgets and configure notifications for actual or forecasted spending. A forecast alert can warn the company before its spending reaches the budget limit.

**Why the other options are incorrect:**
- **A. AWS Cost Explorer:** Analyzes historical spending and forecasts future costs, but AWS Budgets is the direct choice for budget-threshold alerts.
- **C. AWS Pricing Calculator:** Estimates the cost of a planned AWS architecture before deployment.
- **D. AWS CloudTrail:** Records account activity and API calls for auditing.

**Exam tip:** If a question emphasizes a spending limit, budget threshold, or alert for forecasted costs, think **AWS Budgets**.

---

#### Q102 — CLF-C02 Exam Level

A company runs a production application on Amazon EC2. It needs to reduce costs for workloads that run continuously, but it also wants flexibility to change instance families, instance sizes, AWS Regions, and operating systems as its requirements evolve.

Which pricing option is the best fit?

- A. Spot Instances
- B. Standard Reserved Instances
- C. Compute Savings Plans ✅
- D. Dedicated Hosts

**Your answer:** C. Compute Savings Plans

**Correct answer:** C. Compute Savings Plans

**Explanation:** Compute Savings Plans offer lower prices in exchange for committing to a consistent amount of eligible compute usage, measured in dollars per hour, over a 1- or 3-year term. They offer flexibility across EC2 instance families, sizes, Regions, operating systems, and eligible compute services such as AWS Lambda and AWS Fargate.

**Why the other options are incorrect:**
- **A. Spot Instances:** Use spare AWS capacity at discounted prices, but AWS can interrupt the instances.
- **B. Standard Reserved Instances:** Provide discounted pricing with less flexibility for changing instance attributes and configurations.
- **D. Dedicated Hosts:** Provide a dedicated physical server for your use, often for licensing or compliance requirements; they are not the best match for this flexibility-focused discount scenario.

**Exam tip:**
- Flexible compute discounts → **Compute Savings Plans**
- Lowest-cost spare capacity for interruptible workloads → **Spot Instances**
- Dedicated physical server / per-socket or per-core licensing → **Dedicated Hosts**
- Reserved capacity with specific configuration requirements → **Reserved Instances**

---

#### Q103 — CLF-C02 Exam Level

A company wants to identify suspicious activity in its AWS environment, such as unusual API calls, potentially compromised EC2 instances, and communication with known malicious IP addresses. It wants a service that continuously analyzes activity and generates security findings.

Which AWS service should the company use?

- A. AWS CloudTrail
- B. Amazon GuardDuty ✅
- C. AWS WAF
- D. AWS Config

**Correct answer:** B. Amazon GuardDuty

**Explanation:** Amazon GuardDuty is a threat detection service that analyzes supported AWS data sources to identify potentially malicious activity and generate security findings.

**Why the other options are incorrect:**
- **A. AWS CloudTrail:** Records AWS account activity and API calls for auditing. It does not serve as the primary managed threat detection service.
- **C. AWS WAF:** Filters web requests to protect applications from threats such as SQL injection and cross-site scripting (XSS).
- **D. AWS Config:** Tracks resource configurations and evaluates them against configuration rules and compliance requirements.

**Exam tip:**
- Suspicious activity and threat findings → **GuardDuty**
- Who made an API call and when → **CloudTrail**
- Web request filtering → **AWS WAF**
- Resource configuration history and compliance → **AWS Config**

---

#### Q104 — CLF-C02 Exam Level

A company hosts a public-facing web application on AWS. It wants to protect the application against common web exploits, including SQL injection and cross-site scripting (XSS), by inspecting and filtering incoming HTTP and HTTPS requests.

Which AWS service is the best choice?

- A. Amazon GuardDuty
- B. AWS Shield Advanced
- C. AWS WAF ✅
- D. AWS Network Firewall

**Your answer:** C. AWS WAF

**Correct answer:** C. AWS WAF

**Explanation:** AWS WAF is a web application firewall that allows organizations to monitor and control HTTP/HTTPS requests using rules. It can help block common web exploits such as SQL injection and XSS.

**Why the other options are incorrect:**
- **A. Amazon GuardDuty:** Detects potentially malicious activity and security threats in AWS environments.
- **B. AWS Shield Advanced:** Provides enhanced protection against distributed denial-of-service (DDoS) attacks.
- **D. AWS Network Firewall:** Provides network-level traffic filtering and protection for VPCs; it is not the primary choice for web application request filtering.

**Exam tip:**
- SQL injection and XSS → **AWS WAF**
- DDoS attacks → **AWS Shield**
- Suspicious activity and threat findings → **GuardDuty**
- VPC network traffic filtering → **AWS Network Firewall**

---

#### Q105 — CLF-C02 Exam Level

A company needs to ensure that its Amazon EC2 instances are protected by security rules that track connection state. When an instance sends a request to an external server, the response traffic should be allowed automatically without requiring a separate inbound rule for that response.

Which AWS feature meets this requirement?

- A. Network ACLs (NACLs)
- B. Security groups ✅
- C. AWS WAF
- D. Route tables

**Your answer:** B. Security groups

**Correct answer:** B. Security groups

**Explanation:** Security groups are stateful firewalls. When traffic is allowed in one direction, response traffic for that connection is automatically permitted, regardless of the inbound or outbound rule direction.

**Why the other options are incorrect:**
- **A. Network ACLs (NACLs):** Stateless subnet-level controls. You must configure rules for both inbound and outbound traffic as needed, including return traffic.
- **C. AWS WAF:** Filters web application HTTP/HTTPS requests.
- **D. Route tables:** Determine where network traffic is directed; they do not act as stateful firewalls.

**Exam tip:**
- Stateful, resource-level firewall → **Security groups**
- Stateless, subnet-level firewall → **NACLs**
- Traffic routing → **Route tables**

---

#### Q106 — CLF-C02 Exam Level

A company wants to deploy the same infrastructure configuration repeatedly across development, testing, and production environments. It wants to define resources such as Amazon EC2 instances, security groups, and load balancers in templates so that AWS can create and manage them consistently.

Which AWS service should the company use?

- A. AWS Service Catalog
- B. AWS CloudFormation ✅
- C. AWS Config
- D. AWS Systems Manager

**Correct answer:** B. AWS CloudFormation

**Explanation:** AWS CloudFormation uses templates to define and provision AWS infrastructure consistently. It supports repeatable deployments across multiple environments.

**Why the other options are incorrect:**
- **A. AWS Service Catalog:** Provides a catalog of approved IT products for controlled self-service provisioning.
- **C. AWS Config:** Tracks resource configurations and evaluates compliance.
- **D. AWS Systems Manager:** Helps operate and manage AWS and hybrid infrastructure, including automation and operational tasks.

**Exam tip:**
- Infrastructure as code and reusable infrastructure templates → **CloudFormation**
- Approved IT products and controlled self-service → **Service Catalog**
- Configuration history and compliance → **AWS Config**
- Operational management and automation → **Systems Manager**

---

#### Q107 — CLF-C02 Exam Level

A company operates workloads across multiple AWS accounts. Its finance team needs a consolidated view of charges across those accounts, while individual teams should continue managing their own resources.

Which AWS service or feature best meets the financial requirement?

- A. AWS Cost Explorer
- B. AWS Organizations with consolidated billing ✅
- C. AWS Budgets
- D. AWS Control Tower

**Your answer:** B. AWS Organizations with consolidated billing

**Correct answer:** B. AWS Organizations with consolidated billing

**Explanation:** AWS Organizations centrally manages multiple AWS accounts. Consolidated billing combines charges from member accounts into a single management-account billing view and can provide access to eligible volume pricing benefits.

**Why the other options are incorrect:**
- **A. AWS Cost Explorer:** Analyzes and visualizes cost and usage data; it is not the primary feature for consolidating billing across accounts.
- **C. AWS Budgets:** Sets cost or usage thresholds and sends alerts.
- **D. AWS Control Tower:** Helps establish and govern a multi-account AWS environment using a landing zone and guardrails.

**Exam tip:** Multiple AWS accounts plus centralized billing → **AWS Organizations with consolidated billing**.

---

#### Q108 — CLF-C02 Exam Level

A company is moving to AWS and wants to establish a secure, well-governed multi-account environment. It needs a preconfigured landing zone, account setup automation, and guardrails to help maintain governance across accounts.

Which AWS service is the best fit?

- A. AWS Organizations alone
- B. AWS Control Tower ✅
- C. AWS CloudFormation
- D. AWS Trusted Advisor

**Your answer:** B. AWS Control Tower

**Correct answer:** B. AWS Control Tower

**Explanation:** AWS Control Tower helps set up and govern a multi-account AWS environment. It builds on services such as AWS Organizations and provides a landing zone, account provisioning workflows, and guardrails.

**Why the other options are incorrect:**
- **A. AWS Organizations alone:** Centrally manages multiple AWS accounts and organizational units, but does not provide the same integrated landing-zone setup and governance experience.
- **C. AWS CloudFormation:** Provisions infrastructure using templates.
- **D. AWS Trusted Advisor:** Provides recommendations on areas such as cost optimization, security, performance, and fault tolerance.

**Exam tip:**
- Multi-account landing zone and guardrails → **Control Tower**
- Organize and centrally manage accounts → **Organizations**
- Infrastructure as code → **CloudFormation**
- AWS best-practice recommendations → **Trusted Advisor**

---

#### Q109 — CLF-C02 Exam Level

A company wants to migrate a large amount of data from its on-premises data center to Amazon S3. Its internet connection is too slow to transfer the data within the required timeframe. The company wants to use a physical device to transport the data to AWS.

Which AWS service is the best fit?

- A. AWS DataSync
- B. AWS Database Migration Service (AWS DMS)
- C. AWS Snowball Edge ✅
- D. AWS Storage Gateway

**Your answer:** C. AWS Snowball Edge

**Correct answer:** C. AWS Snowball Edge

**Explanation:** AWS Snowball Edge uses physical devices to move large datasets to or from AWS when network transfer is too slow, costly, or impractical.

**Why the other options are incorrect:**
- **A. AWS DataSync:** Transfers data online between supported storage locations.
- **B. AWS DMS:** Migrates databases between supported database platforms.
- **D. AWS Storage Gateway:** Connects on-premises environments with AWS cloud storage through hybrid storage workflows.

**Exam tip:**
- Physical data transfer because of limited bandwidth → **Snowball Edge**
- Online file/object data transfer → **DataSync**
- Database migration → **DMS**
- Hybrid on-premises/cloud storage → **Storage Gateway**

---

#### Q110 — CLF-C02 Exam Level

A company wants to migrate its relational database from an on-premises data center to Amazon RDS. It needs to keep the source database operational during most of the migration and minimize application downtime while data is replicated to the target database.

Which AWS service is the best fit?

- A. AWS DataSync
- B. AWS Database Migration Service (AWS DMS) ✅
- C. AWS Migration Hub
- D. AWS Snowball Edge

**Your answer:** B. AWS Database Migration Service (AWS DMS)

**Correct answer:** B. AWS Database Migration Service (AWS DMS)

**Explanation:** AWS DMS migrates data between supported databases and can perform ongoing replication of changes. This helps keep the target database synchronized while the source remains operational during much of the migration.

**Why the other options are incorrect:**
- **A. AWS DataSync:** Transfers files and object data, not primarily relational database replication.
- **C. AWS Migration Hub:** Tracks migration progress across supported migration tools; it does not perform the database migration itself.
- **D. AWS Snowball Edge:** Physically transfers large datasets when network transfer is impractical; it is not the primary service for ongoing database replication.

**Exam tip:** Database migration and ongoing replication → **AWS DMS**. Migration tracking → **Migration Hub**.

---

#### Q111 — CLF-C02 Exam Level

A company wants to run SQL queries on large amounts of structured data collected from business applications. The data is used for business intelligence reports and analytics rather than day-to-day transactional processing.

Which AWS service is the best fit?

- A. Amazon RDS
- B. Amazon DynamoDB
- C. Amazon Redshift ✅
- D. Amazon ElastiCache

**Your answer:** C. Amazon Redshift

**Correct answer:** C. Amazon Redshift

**Explanation:** Amazon Redshift is a cloud data warehouse optimized for analytical workloads, including complex SQL queries, reporting, and business intelligence across large datasets.

**Why the other options are incorrect:**
- **A. Amazon RDS:** Managed relational databases suited to transactional application workloads.
- **B. Amazon DynamoDB:** Managed NoSQL database designed for low-latency access at scale.
- **D. Amazon ElastiCache:** In-memory caching that speeds up access to frequently requested data.

**Exam tip:**
- Data warehouse and large-scale analytics → **Redshift**
- Managed relational application database → **RDS**
- NoSQL key-value/document database → **DynamoDB**
- In-memory caching → **ElastiCache**

---

#### Q112 — CLF-C02 Exam Level

A company needs a managed NoSQL database for a shopping-cart application. The application must handle millions of requests with consistently low latency, and the company does not want to manage database servers.

Which AWS service is the best fit?

- A. Amazon Aurora
- B. Amazon Redshift
- C. Amazon DynamoDB ✅
- D. Amazon RDS for PostgreSQL

**Your answer:** C. Amazon DynamoDB

**Correct answer:** C. Amazon DynamoDB

**Explanation:** Amazon DynamoDB is a fully managed NoSQL database that supports key-value and document data models and provides low-latency performance at scale without requiring customers to manage database servers.

**Why the other options are incorrect:**
- **A. Amazon Aurora:** A managed relational database compatible with MySQL and PostgreSQL.
- **B. Amazon Redshift:** A data warehouse for analytics and business intelligence.
- **D. Amazon RDS for PostgreSQL:** A managed relational database using SQL and relational tables.

**Exam tip:**
- Managed NoSQL and low-latency access at scale → **DynamoDB**
- Managed relational database → **RDS or Aurora**
- Analytical data warehouse → **Redshift**

---

#### Q113 — CLF-C02 Exam Level

A company runs a critical application on Amazon EC2. It needs block storage that delivers consistent, high I/O performance for a demanding relational database workload.

Which Amazon EBS volume type is the best fit?

- A. General Purpose SSD (gp3)
- B. Throughput Optimized HDD (st1)
- C. Provisioned IOPS SSD (io2) ✅
- D. Cold HDD (sc1)

**Your answer:** C. Provisioned IOPS SSD (io2)

**Correct answer:** C. Provisioned IOPS SSD (io2)

**Explanation:** Provisioned IOPS SSD volumes are designed for I/O-intensive workloads that require high performance and predictable IOPS, such as demanding relational databases.

**Why the other options are incorrect:**
- **A. gp3:** General-purpose SSD for a broad range of workloads at a lower cost than specialized high-IOPS storage.
- **B. st1:** Throughput-optimized HDD suited to frequently accessed, throughput-intensive workloads such as big data and log processing.
- **D. sc1:** Cold HDD intended for infrequently accessed, throughput-oriented data at low cost.

**Exam tip:**
- Critical database with high, predictable IOPS → **io2**
- General-purpose SSD → **gp3**
- High-throughput HDD → **st1**
- Infrequently accessed HDD data → **sc1**

---

#### Q114 — CLF-C02 Exam Level

A company needs to store files that are accessed by multiple Linux EC2 instances running in different Availability Zones. All instances must be able to read and write the same files concurrently. The company wants a managed, shared file system rather than attaching separate block-storage volumes to each instance.

Which AWS service should it choose?

- A. Amazon EBS
- B. Amazon EFS ✅
- C. Amazon S3 Glacier Flexible Retrieval
- D. EC2 instance store

**Your answer:** B. Amazon EFS

**Correct answer:** B. Amazon EFS

**Explanation:** Amazon EFS is a managed, elastic file system that supports shared file access from multiple compute instances. It is commonly used for Linux workloads requiring concurrent access across Availability Zones.

**Why the other options are incorrect:**
- **A. Amazon EBS:** Primarily provides block storage for EC2 instances; a typical EBS volume is attached to one instance at a time, with specific exceptions.
- **C. Amazon S3 Glacier Flexible Retrieval:** An archival storage class for infrequently accessed objects, not a shared file system for active application files.
- **D. EC2 instance store:** Local temporary storage tied to the host; it is not a persistent, shared file system.

**Exam tip:**
- Shared Linux file system across EC2 instances/AZs → **EFS**
- Block storage for an EC2 instance → **EBS**
- Temporary local storage → **Instance store**
- Long-term archival objects → **S3 Glacier storage classes**

---

#### Q115 — CLF-C02 Exam Level

A company wants to run a database that is compatible with MySQL and PostgreSQL. It needs high performance, automated backups, and managed database operations without managing the underlying database infrastructure.

Which AWS service is the best fit?

- A. Amazon DynamoDB
- B. Amazon Aurora ✅
- C. Amazon Redshift
- D. Amazon ElastiCache

**Your answer:** B. Amazon Aurora

**Correct answer:** B. Amazon Aurora

**Explanation:** Amazon Aurora is a managed relational database engine compatible with MySQL and PostgreSQL. AWS manages much of the underlying database infrastructure and supports automated backups.

**Why the other options are incorrect:**
- **A. Amazon DynamoDB:** A NoSQL database for key-value and document workloads.
- **C. Amazon Redshift:** A cloud data warehouse for analytical queries and business intelligence.
- **D. Amazon ElastiCache:** An in-memory caching service that improves data-access speed.

**Exam tip:**
- Managed, high-performance MySQL/PostgreSQL-compatible database → **Aurora**
- NoSQL database → **DynamoDB**
- Data warehouse → **Redshift**
- In-memory cache → **ElastiCache**

---

#### Q116 — CLF-C02 Exam Level

A company wants to ensure that its AWS resources follow internal configuration requirements. It needs to review configuration changes over time and identify resources that violate its configuration rules.

Which AWS service best meets this requirement?

- A. AWS CloudTrail
- B. Amazon GuardDuty
- C. AWS Config ✅
- D. Amazon CloudWatch

**Your answer:** C. AWS Config

**Correct answer:** C. AWS Config

**Explanation:** AWS Config records resource configuration information, tracks changes over time, and evaluates resources against configuration rules to identify compliance issues.

**Why the other options are incorrect:**
- **A. AWS CloudTrail:** Records account activity and API calls, including who performed an action and when.
- **B. Amazon GuardDuty:** Detects potential security threats and malicious activity.
- **D. Amazon CloudWatch:** Monitors metrics, logs, and alarms for operational and application health.

**Exam tip:**
- Resource configuration history and compliance → **AWS Config**
- API activity and auditing → **CloudTrail**
- Threat detection → **GuardDuty**
- Metrics, logs, and alarms → **CloudWatch**

---

#### Q117 — CLF-C02 Exam Level

A company wants to run its application in the AWS Cloud but does not want to manage servers, operating systems, or runtime environments. It only wants to upload its application code and have AWS handle provisioning, load balancing, scaling, and application health monitoring.

Which AWS service is the best fit?

- A. Amazon EC2
- B. AWS Elastic Beanstalk ✅
- C. AWS Lambda
- D. AWS CloudFormation

**Your answer:** B. AWS Elastic Beanstalk

**Correct answer:** B. AWS Elastic Beanstalk

**Explanation:** Elastic Beanstalk simplifies application deployment by provisioning and managing supporting infrastructure, including compute resources, load balancing, scaling, and health monitoring.

**Why the other options are incorrect:**
- **A. Amazon EC2:** Provides virtual servers, but the customer has more responsibility for operating systems and server configuration.
- **C. AWS Lambda:** Runs code in response to events without requiring server management; it is best suited to supported event-driven and serverless workloads.
- **D. AWS CloudFormation:** Provisions infrastructure from templates rather than serving as the primary application deployment platform described here.

**Exam tip:**
- Deploy an application while AWS manages supporting infrastructure → **Elastic Beanstalk**
- Full control over virtual servers → **EC2**
- Run code without managing servers → **Lambda**
- Define infrastructure using templates → **CloudFormation**

---

#### Q118 — CLF-C02 Exam Level

A company wants to monitor CPU utilization on its EC2 instances and receive an alert when CPU usage remains above 80% for a specified period. The company also wants to use that metric to trigger an automated response.

Which AWS service should it use?

- A. AWS CloudTrail
- B. AWS Config
- C. Amazon CloudWatch ✅
- D. Amazon GuardDuty

**Your answer:** C. Amazon CloudWatch

**Correct answer:** C. Amazon CloudWatch

**Explanation:** CloudWatch collects and monitors metrics, logs, and events. CloudWatch alarms evaluate metrics against configured thresholds and can trigger actions, such as sending notifications through Amazon SNS or initiating supported automated responses.

**Why the other options are incorrect:**
- **A. AWS CloudTrail:** Records AWS account activity and API calls.
- **B. AWS Config:** Tracks resource configurations and evaluates compliance.
- **D. Amazon GuardDuty:** Detects potential security threats and suspicious activity.

**Exam tip:**
- CPU metrics, monitoring, and alarms → **CloudWatch**
- API audit history → **CloudTrail**
- Configuration compliance → **AWS Config**
- Threat detection → **GuardDuty**

---

#### Q119 — CLF-C02 Exam Level

A company needs to allow employees to sign in once and access multiple AWS accounts and supported business applications using their existing corporate identity provider. The company wants centralized workforce access management rather than creating separate IAM users in every account.

Which AWS service is the best fit?

- A. AWS IAM Identity Center ✅
- B. AWS Secrets Manager
- C. AWS Organizations
- D. AWS Certificate Manager (ACM)

**Your answer:** A. AWS IAM Identity Center

**Correct answer:** A. AWS IAM Identity Center

**Explanation:** IAM Identity Center provides centralized workforce access, single sign-on (SSO), and permission assignment across multiple AWS accounts and supported applications.

**Why the other options are incorrect:**
- **B. AWS Secrets Manager:** Stores and manages secrets such as database credentials and API keys.
- **C. AWS Organizations:** Centrally manages multiple AWS accounts and supports organizational policies and consolidated billing.
- **D. AWS Certificate Manager (ACM):** Manages SSL/TLS certificates for supported AWS services.

**Exam tip:**
- Workforce SSO across AWS accounts and applications → **IAM Identity Center**
- Centralized multi-account management → **AWS Organizations**
- Passwords, API keys, and secret rotation → **Secrets Manager**
- SSL/TLS certificates → **ACM**

---

#### Q120 — CLF-C02 Exam Level

A company wants to protect sensitive information stored in Amazon S3. It needs to manage encryption keys centrally and control who can use those keys to encrypt and decrypt data.

Which AWS service best meets this requirement?

- A. AWS Secrets Manager
- B. AWS Key Management Service (AWS KMS) ✅
- C. AWS Certificate Manager (ACM)
- D. AWS IAM Identity Center

**Your answer:** B. AWS Key Management Service (AWS KMS)

**Correct answer:** B. AWS Key Management Service (AWS KMS)

**Explanation:** AWS KMS creates and manages cryptographic keys and controls their use through key policies and IAM permissions. It integrates with Amazon S3 encryption features.

**Why the other options are incorrect:**
- **A. AWS Secrets Manager:** Stores and manages sensitive credentials, such as database passwords and API keys.
- **C. AWS Certificate Manager (ACM):** Manages SSL/TLS certificates.
- **D. AWS IAM Identity Center:** Manages workforce access and single sign-on across AWS accounts and supported applications.

**Exam tip:**
- Encryption key management → **KMS**
- Secrets and credential rotation → **Secrets Manager**
- SSL/TLS certificates → **ACM**
- Workforce single sign-on → **IAM Identity Center**

---

#### Q121 — CLF-C02 Exam Level

A healthcare company must provide its auditors with AWS compliance documentation, including reports that help demonstrate how AWS meets applicable security and compliance standards.

The company wants to obtain these documents directly from AWS.

Which AWS service should it use?

- A. AWS Trusted Advisor
- B. AWS Artifact ✅
- C. AWS CloudTrail
- D. AWS Audit Manager

**Your answer:** B. AWS Artifact

**Correct answer:** B. AWS Artifact

**Explanation:** AWS Artifact provides on-demand access to AWS security and compliance documentation, including audit reports and certifications.

**Why the other options are incorrect:**
- **A. AWS Trusted Advisor:** Provides recommendations for cost optimization, security, performance, fault tolerance, and service limits.
- **C. AWS CloudTrail:** Records AWS account activity and API calls.
- **D. AWS Audit Manager:** Collects evidence from your AWS environment to help assess compliance and prepare for audits.

**Exam tip:**
- Need AWS compliance reports? → **AWS Artifact**
- Need to collect evidence for your own compliance audits? → **AWS Audit Manager**
- Need a record of API activity? → **AWS CloudTrail**

---

#### Q122 — CLF-C02 Exam Level

A company is moving its business operations to AWS. Its leadership wants guidance on organizational change, governance, skills, and business outcomes—not just technical migration steps.

Which AWS resource is designed to help organizations plan and accelerate their cloud adoption journey?

- A. AWS Well-Architected Framework
- B. AWS Cloud Adoption Framework (AWS CAF) ✅
- C. AWS Migration Hub
- D. AWS Control Tower

**Your answer:** B. AWS Cloud Adoption Framework (AWS CAF)

**Correct answer:** B. AWS Cloud Adoption Framework (AWS CAF)

**Explanation:** AWS CAF helps organizations develop a structured cloud adoption plan. It considers organizational capabilities, people, governance, business goals, and technology.

**Why the other options are incorrect:**
- **A. AWS Well-Architected Framework:** Reviews workloads against architectural best practices and six pillars.
- **C. AWS Migration Hub:** Helps track migration progress across applications and migration tools.
- **D. AWS Control Tower:** Helps set up and govern a multi-account AWS environment using a landing zone and guardrails.

**Exam tip:**
- Cloud adoption strategy and organizational transformation → **AWS CAF**
- Architecture review and best practices → **Well-Architected Framework**
- Migration tracking → **Migration Hub**
- Multi-account governance setup → **Control Tower**

---

#### Q123 — CLF-C02 Exam Level

A company runs a production application on Amazon EC2. It wants to automatically replace an instance if the instance becomes unhealthy, even when the overall demand for the application has not increased.

Which solution best meets this requirement?

- A. Configure Amazon CloudWatch billing alarms.
- B. Use an EC2 Auto Scaling group with health checks and a minimum capacity of one instance. ✅
- C. Purchase a Compute Savings Plan.
- D. Use AWS Trusted Advisor to monitor instance health.

**Your answer:** B. EC2 Auto Scaling group with health checks and a minimum capacity of one instance

**Correct answer:** B. EC2 Auto Scaling group with health checks and a minimum capacity of one instance

**Explanation:** An EC2 Auto Scaling group can monitor instance health and replace unhealthy instances. A minimum capacity of one helps maintain at least one instance, subject to successful replacement and available capacity.

**Why the other options are incorrect:**
- **A. CloudWatch billing alarms:** Alert on billing or spending thresholds; they do not replace EC2 instances.
- **C. Compute Savings Plan:** Reduces eligible compute costs in exchange for a usage commitment.
- **D. Trusted Advisor:** Provides best-practice recommendations; it is not the mechanism for automatic instance replacement.

**Exam tip:**
- Replace unhealthy EC2 instances → **EC2 Auto Scaling with health checks**
- Distribute traffic across instances → **Elastic Load Balancing**
- Scale capacity based on demand → **EC2 Auto Scaling policies**

---

#### Q124 — CLF-C02 Exam Level

A company is planning to migrate an application to AWS. Before deploying anything, its finance team wants to estimate the expected monthly cost of different AWS services based on its anticipated usage.

Which tool should the company use?

- A. AWS Cost Explorer
- B. AWS Budgets
- C. AWS Pricing Calculator ✅
- D. AWS Trusted Advisor

**Your answer:** C. AWS Pricing Calculator

**Correct answer:** C. AWS Pricing Calculator

**Explanation:** AWS Pricing Calculator estimates the cost of AWS services before deployment. You can model expected usage and compare estimated monthly costs for different architectures.

**Why the other options are incorrect:**
- **A. AWS Cost Explorer:** Analyzes historical spending and helps forecast future costs.
- **B. AWS Budgets:** Sets spending or usage thresholds and sends alerts when defined conditions are met.
- **D. AWS Trusted Advisor:** Provides recommendations for optimizing cost, security, performance, and other areas.

**Exam tip:**
- Estimate costs before deployment → **Pricing Calculator**
- Analyze past costs → **Cost Explorer**
- Set spending alerts → **AWS Budgets**

---

#### Q125 — CLF-C02 Exam Level

A company wants to move a large amount of data from its on-premises data center to AWS. Its internet connection is too slow to transfer the data within the required timeframe. The company also wants to avoid managing the physical infrastructure used for the transfer.

Which AWS service or solution is the best fit?

- A. AWS DataSync
- B. AWS Snowball Edge ✅
- C. AWS Storage Gateway
- D. AWS Database Migration Service (AWS DMS)

**Your answer:** B. AWS Snowball Edge

**Correct answer:** B. AWS Snowball Edge

**Explanation:** AWS Snowball Edge uses a physical device to transfer large volumes of data to AWS when transferring over the network would take too long. AWS provides the device as part of its data transfer service.

**Why the other options are incorrect:**
- **A. AWS DataSync:** Automates and accelerates online data transfers between on-premises storage and AWS over a network.
- **C. AWS Storage Gateway:** Provides hybrid cloud storage, connecting on-premises applications to AWS storage.
- **D. AWS DMS:** Migrates databases, including their data, to AWS or between supported database platforms.

**Exam tip:**
- Too much data for a slow network → **Snowball Edge**
- Online file transfer automation → **DataSync**
- Hybrid on-premises and cloud storage → **Storage Gateway**
- Database migration → **DMS**

---

#### Q126 — CLF-C02 Exam Level

A company wants to protect its AWS account from unauthorized API activity. It needs a service that records who made an API call, what action was performed, and when it occurred, so investigators can review account activity after a security incident.

Which AWS service should it use?

- A. Amazon GuardDuty
- B. AWS CloudTrail ✅
- C. Amazon CloudWatch
- D. AWS Config

**Your answer:** B. AWS CloudTrail

**Correct answer:** B. AWS CloudTrail

**Explanation:** AWS CloudTrail records account activity and API calls. Its event records can help investigators identify who performed an action, what happened, and when it happened.

**Why the other options are incorrect:**
- **A. Amazon GuardDuty:** Detects potential threats by analyzing supported log sources and other security signals.
- **C. Amazon CloudWatch:** Monitors metrics, logs, alarms, and operational performance.
- **D. AWS Config:** Records resource configurations and helps assess configuration compliance.

**Exam tip:**
- Who performed an API action? → **CloudTrail**
- Is there a potential security threat? → **GuardDuty**
- Is an application or resource performing poorly? → **CloudWatch**
- What changed in a resource's configuration? → **AWS Config**

---

#### Q127 — CLF-C02 Exam Level

A company runs a web application on Amazon EC2 instances. It wants to block malicious HTTP requests, including attempts to exploit SQL injection and cross-site scripting (XSS), before those requests reach the application.

Which AWS service should it use?

- A. Amazon GuardDuty
- B. AWS Shield Standard
- C. AWS WAF ✅
- D. AWS CloudTrail

**Your answer:** C. AWS WAF

**Correct answer:** C. AWS WAF

**Explanation:** AWS WAF filters web requests using configurable rules. It helps protect web applications against common web exploits, malicious request patterns, and unwanted traffic.

**Why the other options are incorrect:**
- **A. Amazon GuardDuty:** Detects potential threats and suspicious activity in AWS environments.
- **B. AWS Shield Standard:** Provides automatic protection against common network and transport-layer DDoS attacks.
- **D. AWS CloudTrail:** Records AWS account activity and API calls.

**Exam tip:**
- SQL injection and XSS → **AWS WAF**
- DDoS attacks → **AWS Shield**
- Threat detection → **GuardDuty**
- API activity records → **CloudTrail**

---

#### Q128 — CLF-C02 Exam Level

A company stores confidential customer information in Amazon S3. It wants to encrypt the data at rest and maintain control over the encryption keys, including the ability to manage key permissions and audit key usage.

Which AWS service is the best fit?

- A. AWS Secrets Manager
- B. AWS Key Management Service (AWS KMS) ✅
- C. AWS CloudTrail
- D. AWS Certificate Manager (ACM)

**Your answer:** B. AWS Key Management Service (AWS KMS)

**Correct answer:** B. AWS Key Management Service (AWS KMS)

**Explanation:** AWS KMS creates and manages cryptographic keys and controls their use through permissions and policies. Amazon S3 integrates with KMS for server-side encryption using AWS KMS keys (SSE-KMS). Key usage can be audited through AWS CloudTrail.

**Why the other options are incorrect:**
- **A. AWS Secrets Manager:** Stores, retrieves, and can rotate secrets such as database credentials and API keys.
- **C. AWS CloudTrail:** Records AWS account activity, including relevant KMS API activity; it does not manage encryption keys.
- **D. AWS Certificate Manager (ACM):** Provisions and manages SSL/TLS certificates for supported services.

**Exam tip:**
- Manage encryption keys → **KMS**
- Store and rotate credentials → **Secrets Manager**
- Manage SSL/TLS certificates → **ACM**
- Audit API activity → **CloudTrail**

---

#### Q129 — CLF-C02 Exam Level

A company wants to run a containerized application on AWS without provisioning or managing the underlying servers. It wants AWS to manage the server infrastructure while the company focuses on its containers.

Which service is the best fit?

- A. Amazon EC2
- B. Amazon ECS on AWS Fargate ✅
- C. Amazon EKS on self-managed EC2 worker nodes
- D. AWS Elastic Beanstalk

**Your answer:** B. Amazon ECS on AWS Fargate

**Correct answer:** B. Amazon ECS on AWS Fargate

**Explanation:** Amazon ECS orchestrates containers, while AWS Fargate provides serverless compute capacity for running them. You don't need to provision or manage EC2 instances for the container workloads.

**Why the other options are incorrect:**
- **A. Amazon EC2:** You manage the virtual servers and their operating systems.
- **C. Amazon EKS on self-managed EC2 worker nodes:** You manage the EC2 worker-node infrastructure.
- **D. AWS Elastic Beanstalk:** Simplifies application deployment and infrastructure management, but it is not the most direct answer to this container-specific requirement.

**Exam tip:**
- Containers without managing servers → **ECS with Fargate** or **EKS with Fargate**, depending on the orchestration requirement.
- Containers on servers you manage → **ECS/EKS on EC2**.
- Simplified application deployment → **Elastic Beanstalk**.

---

#### Q130 — CLF-C02 Exam Level

A company needs to recover from a regional disaster with minimal downtime and minimal data loss. It is willing to pay more to maintain a scaled-down but fully functional copy of its environment in a second AWS Region.

- A. Backup and restore
- B. Pilot light
- C. Warm standby ✅
- D. Multi-site active-active

**Your answer:** C. Warm standby

**Correct answer:** C. Warm standby

**Explanation:** Warm standby maintains a scaled-down, fully functional environment in another AWS Region. During a disaster, the company can scale up this environment to support production workloads.

**Exam notes:**
- Backup and restore: Restore data and rebuild resources after a disaster.
- Pilot light: Keep essential core resources running and start additional resources when needed.
- Warm standby: Maintain a scaled-down, functional environment that can be scaled up quickly.
- Multi-site active-active: Run active workloads across multiple locations simultaneously.

**Key takeaway:** "Scaled-down but fully functional" is the key phrase for Warm standby.

---

#### Q131 — CLF-C02 Exam Level

A company wants an alert whenever its estimated monthly AWS charges exceed $500, without managing infrastructure.

- A. AWS Cost Explorer
- B. AWS CloudTrail
- C. AWS Budgets ✅
- D. Amazon CloudWatch

**Your answer:** C. AWS Budgets

**Correct answer:** C. AWS Budgets

**Explanation:** AWS Budgets allows users to set custom cost and usage thresholds and configure notifications when actual or forecasted spending exceeds those thresholds.

**Exam notes:**
- AWS Budgets: Set spending limits and receive cost or usage alerts.
- AWS Cost Explorer: Analyze and visualize spending trends.
- AWS CloudTrail: Track account API activity.
- Amazon CloudWatch: Monitor operational metrics, logs, and alarms.

**Key takeaway:** For budget thresholds and spending alerts, choose AWS Budgets.

---

#### Q132 — CLF-C02 Exam Level

An application on an EC2 instance needs access to S3 without storing AWS access keys in its code.

- A. Store the access keys in a configuration file on the EC2 instance.
- B. Use an IAM role attached to the EC2 instance with the required S3 permissions. ✅
- C. Create an IAM user and hardcode its access keys in the application.
- D. Make the S3 bucket publicly accessible.

**Your answer:** B. Use an IAM role attached to the EC2 instance with the required S3 permissions.

**Correct answer:** B. Use an IAM role attached to the EC2 instance with the required S3 permissions.

**Explanation:** An IAM role provides temporary security credentials to an EC2 instance. The application can use these credentials to access AWS services without embedding permanent access keys in the code.

**Exam notes:**
- IAM role: Provides temporary credentials to AWS services or workloads.
- Least privilege: Grant only the permissions required.
- IAM user access keys: Avoid hardcoding them in application code.
- Public access: Do not expose private S3 data to solve an access problem.

**Key takeaway:** For applications running on EC2 that need AWS service access, prefer an IAM role over hardcoded access keys.

---

#### Q133 — CLF-C02 Exam Level

A company needs a relational database with automated backups, software patching, and managed maintenance so its team can focus on application development.

- A. Amazon EC2 with a self-managed database
- B. Amazon DynamoDB
- C. Amazon RDS ✅
- D. Amazon S3

**Your answer:** C. Amazon RDS

**Correct answer:** C. Amazon RDS

**Explanation:** Amazon RDS is a managed relational database service that automates many routine administrative tasks, including backups, software patching, and maintenance.

**Exam notes:**
- Amazon RDS: Managed relational databases.
- Amazon Aurora: AWS relational database engine compatible with MySQL and PostgreSQL.
- Amazon DynamoDB: Managed NoSQL database.
- Amazon EC2: Provides virtual servers; a self-managed database requires more administrative work.
- Amazon S3: Object storage.

**Key takeaway:** Choose Amazon RDS when you need a managed relational database and want to reduce administrative overhead.

---

#### Q134 — CLF-C02 Exam Level

A company needs to inspect incoming HTTP and HTTPS requests and block web exploits such as SQL injection and cross-site scripting (XSS).

- A. AWS Shield
- B. AWS WAF ✅
- C. Amazon GuardDuty
- D. AWS CloudTrail

**Your answer:** B. AWS WAF

**Correct answer:** B. AWS WAF

**Explanation:** AWS WAF is a web application firewall that filters HTTP and HTTPS requests using configurable rules to help protect applications from common web exploits.

**Exam notes:**
- AWS WAF: Filters web requests and helps protect against SQL injection and XSS.
- AWS Shield: Protects against DDoS attacks.
- Amazon GuardDuty: Detects potential security threats.
- AWS CloudTrail: Records account activity and API calls.

**Key takeaway:** Web request filtering and common web exploits → AWS WAF.

---

#### Q135 — CLF-C02 Exam Level

A company needs centralized management of encryption keys, key permissions, and auditing of key usage.

- A. AWS Secrets Manager
- B. AWS Key Management Service (AWS KMS) ✅
- C. AWS CloudTrail
- D. AWS Artifact

**Your answer:** B. AWS Key Management Service (AWS KMS)

**Correct answer:** B. AWS Key Management Service (AWS KMS)

**Explanation:** AWS KMS helps create and manage cryptographic keys used to encrypt data. Key policies and IAM policies control access, while AWS CloudTrail records relevant API activity for auditing.

**Exam notes:**
- AWS KMS: Encryption key management.
- AWS Secrets Manager: Secrets such as passwords, API keys, and database credentials.
- AWS CloudTrail: Records API activity for auditing.
- AWS Artifact: Compliance reports and agreements.

**Key takeaway:** Encryption key management → AWS KMS.

---

#### Q136 — CLF-C02 Exam Level

A company wants to deploy an application across multiple Availability Zones and continue serving users if one Availability Zone becomes unavailable.

- A. Amazon EC2 Auto Scaling and Elastic Load Balancing (ELB) ✅
- B. A single large EC2 instance in one Availability Zone
- C. Amazon S3 and AWS CloudTrail
- D. Amazon Route 53 only

**Your answer:** A. Amazon EC2 Auto Scaling and Elastic Load Balancing (ELB)

**Correct answer:** A. Amazon EC2 Auto Scaling and Elastic Load Balancing (ELB)

**Explanation:** Elastic Load Balancing distributes incoming traffic across healthy instances. Amazon EC2 Auto Scaling maintains the desired capacity and can replace unhealthy instances. Deploying instances across multiple Availability Zones improves availability.

**Exam notes:**
- Elastic Load Balancing (ELB): Distributes incoming traffic across targets.
- EC2 Auto Scaling: Maintains desired capacity and scales or replaces instances as configured.
- Multiple Availability Zones: Improves fault tolerance and availability.
- Health checks: Help identify unhealthy targets or instances.

**Key takeaway:** High availability across Availability Zones → ELB + EC2 Auto Scaling with a multi-AZ deployment.

---

#### Q137 — CLF-C02 Exam Level

A company needs to retain infrequently accessed data for several years at the lowest possible storage cost. Retrieval can take several hours.

- A. S3 Standard
- B. S3 Standard-IA
- C. S3 Glacier Deep Archive ✅
- D. S3 Intelligent-Tiering

**Your answer:** C. S3 Glacier Deep Archive

**Correct answer:** C. S3 Glacier Deep Archive

**Explanation:** S3 Glacier Deep Archive is designed for long-term data archiving at very low storage cost. Retrieval generally takes hours rather than providing immediate access.

**Exam notes:**
- S3 Standard: Frequently accessed data.
- S3 Intelligent-Tiering: Data with changing or unpredictable access patterns.
- S3 Standard-IA: Infrequently accessed data requiring fast access.
- S3 Glacier Deep Archive: Long-term archival with the lowest storage cost among these options.

**Key takeaway:** Long-term archive + lowest storage cost + hours-long retrieval → S3 Glacier Deep Archive.

---

#### Q138 — CLF-C02 Exam Level

A company wants recommendations for its AWS environment covering security, fault tolerance, service limits, performance, and cost optimization.

- A. AWS Trusted Advisor ✅
- B. AWS CloudTrail
- C. Amazon CloudWatch
- D. AWS Artifact

**Your answer:** A. AWS Trusted Advisor

**Correct answer:** A. AWS Trusted Advisor

**Explanation:** AWS Trusted Advisor provides checks and recommendations that help customers follow AWS best practices and improve their AWS environments.

**Exam notes:**
- AWS Trusted Advisor: Best-practice recommendations.
- AWS CloudTrail: Records account activity and API calls.
- Amazon CloudWatch: Monitors metrics, logs, and alarms.
- AWS Artifact: Provides compliance reports and agreements.

**Key takeaway:** AWS best-practice checks and recommendations → AWS Trusted Advisor.

---

#### Q139 — CLF-C02 Exam Level

A company needs to manage multiple AWS accounts centrally, group accounts by department, apply policies across account groups, and consolidate billing.

- A. AWS IAM Identity Center
- B. AWS Organizations ✅
- C. AWS Control Tower
- D. AWS Config

**Your answer:** B. AWS Organizations

**Correct answer:** B. AWS Organizations

**Explanation:** AWS Organizations centrally manages multiple AWS accounts. Organizational units (OUs) group accounts, service control policies (SCPs) define maximum available permissions, and consolidated billing combines charges across accounts.

**Exam notes:**
- AWS Organizations: Multi-account management and consolidated billing.
- Organizational units (OUs): Group accounts by department or purpose.
- Service control policies (SCPs): Set permission guardrails for accounts; they do not grant permissions.
- AWS IAM Identity Center: Centralized workforce access.
- AWS Control Tower: Establishes and governs a multi-account landing zone.
- AWS Config: Tracks resource configurations and compliance.

**Key takeaway:** Centralized multi-account management → AWS Organizations.

---

#### Q140 — CLF-C02 Exam Level

A company needs to transfer a large amount of data from an on-premises data center to AWS. Its internet connection is slow, so online transfer would take several weeks. It wants to use a physical device.

- A. AWS DataSync
- B. AWS Database Migration Service (AWS DMS)
- C. AWS Snowball Edge ✅
- D. AWS Storage Gateway

**Your answer:** C. AWS Snowball Edge

**Correct answer:** C. AWS Snowball Edge

**Explanation:** AWS Snowball Edge provides physical devices for transferring large datasets to and from AWS when network transfer is impractical.

**Exam notes:**
- AWS Snowball Edge: Physical data transfer devices for large datasets.
- AWS DataSync: Automated online data transfers.
- AWS DMS: Database migration.
- AWS Storage Gateway: Hybrid on-premises and cloud storage integration.

**Key takeaway:** Large data transfer + slow internet + physical device → AWS Snowball Edge.

---

#### Q141 — CLF-C02 Exam Level

A company wants employees to use single sign-on to access multiple AWS accounts and supported business applications with their existing corporate identities.

- A. AWS Organizations
- B. AWS IAM Identity Center ✅
- C. AWS Secrets Manager
- D. AWS Key Management Service (AWS KMS)

**Your answer:** B. AWS IAM Identity Center

**Correct answer:** B. AWS IAM Identity Center

**Explanation:** AWS IAM Identity Center centrally manages workforce access to multiple AWS accounts and supported applications, reducing the need for separate IAM users in each account.

**Exam notes:**
- AWS IAM Identity Center: Workforce access and single sign-on.
- AWS Organizations: Multi-account management and consolidated billing.
- AWS Secrets Manager: Stores and manages secrets.
- AWS KMS: Manages encryption keys.

**Key takeaway:** Workforce SSO across AWS accounts and applications → AWS IAM Identity Center.

---

#### Q142 — CLF-C02 Exam Level

A company wants to discover and classify sensitive information, such as PII, stored in Amazon S3 and receive findings when sensitive data is discovered.

- A. Amazon GuardDuty
- B. Amazon Macie ✅
- C. AWS Security Hub
- D. AWS CloudTrail

**Your answer:** B. Amazon Macie

**Correct answer:** B. Amazon Macie

**Explanation:** Amazon Macie uses machine learning and pattern matching to discover and help protect sensitive data in Amazon S3, including personally identifiable information (PII).

**Exam notes:**
- Amazon Macie: Sensitive data discovery in S3.
- Amazon GuardDuty: Threat detection.
- AWS Security Hub: Centralized security findings and posture management.
- AWS CloudTrail: Records API activity.

**Key takeaway:** Sensitive data discovery and classification in S3 → Amazon Macie.

---

#### Q143 — CLF-C02 Exam Level

A company wants to decouple application components by sending messages to a queue so another component can process them later, even if the receiver is temporarily unavailable.

- A. Amazon SNS
- B. Amazon SQS ✅
- C. Amazon EventBridge
- D. Amazon Kinesis

**Your answer:** B. Amazon SQS

**Correct answer:** B. Amazon SQS

**Explanation:** Amazon SQS is a managed message queuing service. It stores messages until consumers retrieve and process them, allowing application components to operate independently.

**Exam notes:**
- Amazon SQS: Message queues and asynchronous processing.
- Amazon SNS: Publish/subscribe notifications to multiple subscribers.
- Amazon EventBridge: Event routing between applications and services.
- Amazon Kinesis: Streaming data collection and processing.

**Key takeaway:** Queue messages for later processing → Amazon SQS.

---

#### Q144 — CLF-C02 Exam Level

A company needs to send one notification to multiple subscribers, including an email address, a Lambda function, and an SQS queue, whenever a new order is placed.

- A. Amazon SQS
- B. Amazon SNS ✅
- C. Amazon EventBridge
- D. Amazon CloudWatch

**Your answer:** B. Amazon SNS

**Correct answer:** B. Amazon SNS

**Explanation:** Amazon SNS uses a publish/subscribe model. A publisher sends a message to an SNS topic, which delivers notifications to its subscribed endpoints.

**Exam notes:**
- Amazon SNS: Push notifications to multiple subscribers.
- Amazon SQS: Stores messages in a queue for consumers to process.
- Amazon EventBridge: Routes events based on rules.
- Amazon CloudWatch: Monitors metrics, logs, and alarms.

**Key takeaway:** One message distributed to multiple subscribers → Amazon SNS.

---

#### Q145 — CLF-C02 Exam Level

A company wants code to run automatically when an event occurs, such as an S3 file upload, without provisioning servers. It wants to pay based on compute usage.

- A. Amazon EC2
- B. AWS Lambda ✅
- C. Amazon ECS on Amazon EC2
- D. AWS Elastic Beanstalk

**Your answer:** B. AWS Lambda

**Correct answer:** B. AWS Lambda

**Explanation:** AWS Lambda is a serverless compute service that runs code in response to events. AWS manages the underlying infrastructure, and charges are based on requests and execution duration under its applicable pricing model.

**Exam notes:**
- AWS Lambda: Event-driven, serverless code execution.
- Amazon EC2: Virtual servers requiring infrastructure management.
- Amazon ECS on Amazon EC2: Container orchestration with customer-managed EC2 infrastructure.
- AWS Elastic Beanstalk: Simplifies application deployment and environment management.

**Key takeaway:** Run code in response to events without managing servers → AWS Lambda.

---

#### Q146 — CLF-C02 Exam Level

A company wants to migrate its existing on-premises VMware virtual machines to AWS with minimal application changes and automate the server migration process.

- A. AWS Database Migration Service (AWS DMS)
- B. AWS Application Migration Service (AWS MGN) ✅
- C. AWS DataSync
- D. AWS Snowball Edge

**Your answer:** B. AWS Application Migration Service (AWS MGN)

**Correct answer:** B. AWS Application Migration Service (AWS MGN)

**Explanation:** AWS MGN automates the migration of physical, virtual, and cloud servers to AWS. It uses continuous block-level replication and helps launch migrated workloads as EC2 instances with minimal changes.

**Exam notes:**
- AWS MGN: Lift-and-shift server migration.
- AWS DMS: Database migration.
- AWS DataSync: Online data transfer.
- AWS Snowball Edge: Physical data transfer for large datasets.

**Key takeaway:** Migrate entire servers to AWS with minimal application changes → AWS MGN.

---

#### Q147 — CLF-C02 Exam Level

A company needs to collect security findings from multiple AWS services and supported third-party security products in one place to obtain a centralized view of its security posture.

- A. Amazon GuardDuty
- B. Amazon Macie
- C. AWS Security Hub ✅
- D. AWS CloudTrail

**Your answer:** C. AWS Security Hub

**Correct answer:** C. AWS Security Hub

**Explanation:** AWS Security Hub provides centralized security findings and helps assess an AWS environment against supported security standards and best practices.

**Exam notes:**
- AWS Security Hub: Centralized security findings and security posture management.
- Amazon GuardDuty: Threat detection.
- Amazon Macie: Sensitive data discovery in S3.
- AWS CloudTrail: Records API activity for auditing.

**Key takeaway:** Centralized security findings and posture → AWS Security Hub.

---

#### Q148 — CLF-C02 Exam Level

A company needs a private, dedicated network connection between its on-premises data center and Amazon VPC, with more consistent network performance than a typical internet connection.

- A. AWS Site-to-Site VPN
- B. AWS Direct Connect ✅
- C. Amazon CloudFront
- D. Amazon Route 53

**Your answer:** B. AWS Direct Connect

**Correct answer:** B. AWS Direct Connect

**Explanation:** AWS Direct Connect establishes a dedicated network connection from an on-premises environment to AWS, helping provide more consistent network performance.

**Exam notes:**
- AWS Direct Connect: Dedicated network connection to AWS.
- AWS Site-to-Site VPN: Encrypted connection over the internet.
- Amazon CloudFront: Content delivery network (CDN).
- Amazon Route 53: DNS and domain-routing service.

**Key takeaway:** Dedicated network connection to AWS → AWS Direct Connect.

---

#### Q149 — CLF-C02 Exam Level

A company needs a highly available relational database with automatic failover to a standby database in another Availability Zone. The standby should not serve read traffic during normal operation.

- A. Read replica
- B. Multi-AZ deployment ✅
- C. Single-AZ deployment
- D. Amazon DynamoDB global tables

**Your answer:** B. Multi-AZ deployment

**Correct answer:** B. Multi-AZ deployment

**Explanation:** Amazon RDS Multi-AZ deployments maintain a standby database in another Availability Zone for high availability and automatic failover. The standby is not intended to serve read traffic.

**Exam notes:**
- RDS Multi-AZ: High availability and automatic failover.
- Read replicas: Primarily scale read workloads.
- Single-AZ: No standby in another Availability Zone.
- DynamoDB global tables: Multi-Region replication for NoSQL.

**Key takeaway:** RDS high availability with automatic failover → Multi-AZ deployment.

---

#### Q150 — CLF-C02 Exam Level

A company wants to deploy an application quickly without directly managing infrastructure. It wants AWS to handle environment provisioning, load balancing, and application health monitoring.

- A. Amazon EC2
- B. AWS Elastic Beanstalk ✅
- C. AWS Lambda
- D. Amazon EKS

**Your answer:** B. AWS Elastic Beanstalk

**Correct answer:** B. AWS Elastic Beanstalk

**Explanation:** Elastic Beanstalk simplifies application deployment and manages the environment's underlying AWS resources. Developers retain control over their application code while AWS handles much of the environment management.

**Exam notes:**
- AWS Elastic Beanstalk: Managed application deployment and environment management.
- AWS Lambda: Serverless, event-driven code execution.
- Amazon EC2: Virtual servers.
- Amazon EKS: Managed Kubernetes service.

**Key takeaway:** Deploy application code without managing the underlying environment directly → AWS Elastic Beanstalk.

---

#### Q151 — CLF-C02 Exam Level

A company wants to query data stored in Amazon S3 using SQL without loading it into a database or data warehouse. It prefers a serverless service with pricing based on data scanned.

- A. Amazon Redshift
- B. Amazon Athena ✅
- C. Amazon RDS
- D. Amazon OpenSearch Service

**Your answer:** B. Amazon Athena

**Correct answer:** B. Amazon Athena

**Explanation:** Amazon Athena is a serverless query service that analyzes data stored in Amazon S3 using SQL without requiring the data to be loaded into a separate database.

**Exam notes:**
- Amazon Athena: Serverless SQL queries on S3 data.
- Amazon Redshift: Data warehousing and analytics.
- Amazon RDS: Managed relational databases.
- Amazon OpenSearch Service: Search and log analytics.

**Key takeaway:** Run SQL directly on data in S3 → Amazon Athena.

---

#### Q152 — CLF-C02 Exam Level

A company wants to allow only HTTPS traffic to its Application Load Balancer and block common web attacks, such as SQL injection.

- A. Configure AWS Shield and AWS CloudTrail.
- B. Configure an Application Load Balancer security group to allow HTTPS traffic and associate AWS WAF with the Application Load Balancer. ✅
- C. Configure Amazon GuardDuty and Amazon Macie.
- D. Configure AWS Trusted Advisor and AWS Config.

**Your answer:** B. Configure an Application Load Balancer security group to allow HTTPS traffic and associate AWS WAF with the Application Load Balancer.

**Correct answer:** B. Configure an Application Load Balancer security group to allow HTTPS traffic and associate AWS WAF with the Application Load Balancer.

**Explanation:** The security group controls network-level access to the load balancer, while AWS WAF filters HTTP and HTTPS requests to help block common web exploits.

**Exam notes:**
- Security groups: Control network traffic to AWS resources.
- AWS WAF: Filters web requests and helps protect against common web attacks.
- AWS Shield: DDoS protection.
- Amazon GuardDuty: Threat detection.
- Amazon Macie: Sensitive data discovery.

**Key takeaway:** HTTPS network access control + web attack filtering → Security group + AWS WAF.
--- 
## Rapid-Fire Review
 
| If the question says… | Think… |
|---|---|
| Migrate a **database**, minimal downtime, ongoing replication | **AWS DMS** (+ SCT for schema conversion) |
| Move **files** online and keep them in sync | **AWS DataSync** |
| Huge data, slow network, physical device | **Snowball Edge** |
| On-prem app + local cache + cloud storage | **Storage Gateway** (file gateway) |
| Discover on-prem servers & dependencies | **Application Discovery Service** |
| Track migration progress | **Migration Hub** |
| Managed relational database | **RDS** (Aurora for MySQL/PostgreSQL-compatible, high performance) |
| Key-value NoSQL, millisecond latency at scale | **DynamoDB** |
| Data warehouse / complex analytical SQL | **Redshift** |
| Persistent block storage for EC2 + backups | **EBS** + **EBS snapshots** |
| Critical database, very high IOPS | **EBS io2** |
| Temporary, recreatable data, fastest local disk | **EC2 Instance Store** |
| Shared Linux file system across instances/AZs | **EFS** |
| Windows, SMB, Active Directory | **FSx for Windows File Server** |
| Old object versions recoverable | **S3 Versioning** |
| Auto-transition S3 objects to cheaper classes | **S3 Lifecycle** |
| Cheapest archive, retrieval in ~12 h | **S3 Glacier Deep Archive** |
| Lower latency for global users, edge caching | **CloudFront** |
| Change the **number** of instances | **EC2 Auto Scaling** |
| Decide **where traffic goes** / health checks | **ELB** |
| Run short event-driven code, no servers | **Lambda** |
| Containers, no servers to manage | **ECS + Fargate** |
| Control over the OS | **EC2** |
| 1–3 year commitment + flexibility across families/Regions | **Compute Savings Plans** |
| Interruptible, lowest cost | **Spot** |
| Unpredictable, no commitment, no interruptions | **On-Demand** |


Cover the right column and test yourself.

| Clue in the question | Answer |
|---|---|
| Virtual server in AWS | **EC2** |
| Full control over OS | **EC2** |
| Template to launch an instance | **AMI** |
| Add more instances | **Horizontal scaling** |
| Make one instance bigger | **Vertical scaling** |
| Automatically add/remove instances | **EC2 Auto Scaling** |
| Distribute traffic across instances | **ELB** |
| Survive an AZ outage | **Multi-AZ + ELB** |
| Event + short code + no servers | **Lambda** |
| Containers + no server management | **ECS + Fargate** |
| Containers + manage the hosts | **ECS on EC2** |
| Interruptible + cheapest | **Spot** |
| Predictable + 1–3 years | **Reserved / Savings Plans** |
| Short-term + no commitment | **On-Demand** |
| Dedicated physical server | **Dedicated Host** |
| CPU alarm / metrics | **CloudWatch** |
| Who did what, when | **CloudTrail** |
| Config history + compliance | **AWS Config** |
| Cost/security/performance recommendations | **Trusted Advisor** |
| Preapproved product catalog | **Service Catalog** |
| Many accounts, OUs, SCPs | **Organizations** |
| DNS + routing policies | **Route 53** |
| AWS secures hardware; you patch guest OS | **Shared Responsibility Model** |

---

## Common Exam Traps

1. **Auto Scaling vs. ELB.** Auto Scaling = *how many*. ELB = *where traffic goes*. Questions often need **both**.
2. **Vertical vs. horizontal.** "Bigger instance" is vertical; "more instances" is horizontal. Don't let "scaling" alone push you to Auto Scaling.
3. **Spot vs. Reserved.** Words like *interrupted*, *flexible*, *batch* → Spot. Words like *steady*, *predictable*, *1 or 3 years* → Reserved.
4. **CloudWatch vs. CloudTrail vs. Config.** Performance → CloudWatch. Activity/audit → CloudTrail. Configuration state → Config.
5. **Lambda vs. Fargate.** Both are serverless. Lambda = event-driven *functions*; Fargate = *containers*.
6. **Same-AZ solutions are never highly available.** Look for **multiple AZs**.
7. **Shared responsibility depends on the service.** EC2 → you patch the guest OS. Managed services shift more to AWS.
8. **Recommending ≠ doing.** Trusted Advisor recommends; CloudTrail records; SCPs limit. None of them automatically fix or grant things.
9. **Absolute wording is a red flag.** Options saying *always*, *every*, or *without limitations* (like Q25-A and Q26-D) are usually wrong.
