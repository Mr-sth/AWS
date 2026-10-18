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

## 4. Rapid-Fire Review

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

## 5. Common Exam Traps

1. **Auto Scaling vs. ELB.** Auto Scaling = *how many*. ELB = *where traffic goes*. Questions often need **both**.
2. **Vertical vs. horizontal.** "Bigger instance" is vertical; "more instances" is horizontal. Don't let "scaling" alone push you to Auto Scaling.
3. **Spot vs. Reserved.** Words like *interrupted*, *flexible*, *batch* → Spot. Words like *steady*, *predictable*, *1 or 3 years* → Reserved.
4. **CloudWatch vs. CloudTrail vs. Config.** Performance → CloudWatch. Activity/audit → CloudTrail. Configuration state → Config.
5. **Lambda vs. Fargate.** Both are serverless. Lambda = event-driven *functions*; Fargate = *containers*.
6. **Same-AZ solutions are never highly available.** Look for **multiple AZs**.
7. **Shared responsibility depends on the service.** EC2 → you patch the guest OS. Managed services shift more to AWS.
8. **Recommending ≠ doing.** Trusted Advisor recommends; CloudTrail records; SCPs limit. None of them automatically fix or grant things.
9. **Absolute wording is a red flag.** Options saying *always*, *every*, or *without limitations* (like Q25-A and Q26-D) are usually wrong.
