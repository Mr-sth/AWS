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
