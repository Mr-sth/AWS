# Module 2 — Summary

### Compute: 
- The processing power used to run applications and workloads.**

| Concept | Simple Meaning |
|---|---|
| **Amazon EC2** | Virtual servers in the AWS Cloud |
| **EC2 Instance** | One virtual server |
| **Instance Type** | Determines CPU, memory, networking, etc. |
| **AMI** | Template used to launch an EC2 instance |
| **Amazon EC2 Auto Scaling** | Automatically adds or removes EC2 instances based on demand |
| **Elastic Load Balancing (ELB)** | Distributes incoming traffic across resources |
| **AWS Lambda** | Runs code without managing servers |

---

### Important Distinction

Think of **EC2** like renting a computer:

> AWS provides the physical infrastructure → you get a virtual server → you configure and run your application on it.

For EC2, **you are responsible for**:
- Guest operating system
- Applications
- Configurations

**AWS is responsible for**:
- Physical servers
- Data centers
- Underlying physical infrastructure

> 💡 **Remember:** EC2 = **AWS manages the hardware, you manage the virtual server.**


---
# Questions

### Question 1 — Module 2

What is an **Amazon EC2 instance**?

* A. A physical server located in an AWS data center
* **B. A virtual server that runs in the AWS Cloud**
* C. A database used to store application data
* D. A network connection between two AWS Regions

### Answer

**B. A virtual server that runs in the AWS Cloud**

### Notes

* **EC2 = Virtual server**
* An EC2 instance is essentially a **virtual computer in AWS**.
* You can choose:

  * CPU
  * Memory
  * Storage
  * Operating system
  * Network configuration

> 💡 **Remember:** Your laptop = physical computer → **EC2 instance = virtual computer in AWS**

---
### Question 2 — Module 2
A company launches an EC2 instance and needs to install and manage the operating system and applications running on it.

**Who is primarily responsible for managing the guest operating system on the EC2 instance?**

* A. AWS
* **B. The customer**
* C. The internet service provider
* D. The AWS Region

### Answer

**B. The customer**

### Notes

* AWS manages the **underlying physical infrastructure**.
* The customer manages the **guest OS, applications, and configurations**.
* This is part of the **Shared Responsibility Model**.

> 💡 **Remember:** 🏢 AWS → Physical infrastructure | 💻 Customer → OS + Applications + Configuration

---
### Question 3 — Module 2

A company is running an application on one EC2 instance. During busy periods, the application receives much more traffic and needs additional computing capacity.

**Which approach means adding more EC2 instances to handle the increased workload?**

* A. Vertical scaling
* **B. Horizontal scaling**
* C. Data encryption
* D. Load balancing

### Answer

**B. Horizontal scaling**

### Notes

* **Horizontal scaling = Add more machines**

  * 1 EC2 → 3 EC2 → 10 EC2
* **Vertical scaling = Make one machine bigger**

  * Small EC2 → Larger EC2 with more CPU/RAM

> 💡 **Remember:** ↔️ Horizontal = **More machines** | ↕️ Vertical = **Bigger machine**

---
### Question 4 — Module 2

An application is running on an EC2 instance with **2 vCPUs and 4 GB RAM**. The company changes it to an instance with **8 vCPUs and 16 GB RAM**.

**What type of scaling is this?**

* A. Horizontal scaling
* **B. Vertical scaling**
* C. Automatic scaling
* D. Geographic scaling

### Answer

**B. Vertical scaling**

### Notes

* **Horizontal scaling** → Add more instances.
* **Vertical scaling** → Increase the size/power of one instance.
* Here, the company increases **CPU and RAM of the same instance**.

> 💡 **Remember:** ↔️ Horizontal = **More instances** | ↕️ Vertical = **Bigger instance**

---
### Question 5 — Module 2

A website normally runs on **2 EC2 instances**, but during a sale it needs **10 EC2 instances**. After the sale, demand drops and it goes back to 2 instances.

**Which AWS service is designed to automatically add or remove EC2 instances based on demand?**

* **A. Amazon EC2 Auto Scaling**
* B. Amazon S3
* C. AWS Lambda
* D. Amazon RDS

### Answer

**A. Amazon EC2 Auto Scaling**

### Notes

* **EC2 Auto Scaling** automatically adjusts the **number of EC2 instances** based on demand.
* Normal traffic → **2 instances**
* High traffic → **10 instances**
* Traffic drops → **back to 2 instances**
* This is mainly **horizontal scaling**.

> 💡 **Remember:** Auto Scaling = **Automatically add/remove**

---
### Question 6 — Module 2

A company has multiple EC2 instances running the same web application. It wants incoming user requests to be distributed across those instances so that no single instance receives all the traffic.

**Which AWS service should it use?**

* A. Amazon EC2 Auto Scaling
* **B. Elastic Load Balancing (ELB)**
* C. AWS Lambda
* D. Amazon S3

### Answer

**B. Elastic Load Balancing (ELB)**

### Notes

* **ELB** distributes incoming traffic across multiple EC2 instances.
* It helps prevent one instance from receiving **all the traffic**.
* **Auto Scaling** → changes the **number of instances**.
* **ELB** → distributes **incoming traffic**.

> 💡 **Remember:** Auto Scaling = **How many instances?** | ELB = **Where does the traffic go?**

---
### Question 7 — Module 2

A developer has a small piece of code that should run whenever an event occurs. The developer does **not** want to provision or manage servers.

**Which AWS service is the best fit?**

* A. Amazon EC2
* B. Amazon RDS
* **C. AWS Lambda**
* D. Amazon ECS

### Answer

**C. AWS Lambda**

### Notes

* **AWS Lambda** runs code without you managing servers.
* You provide the **code**, and AWS manages the underlying compute infrastructure.
* **EC2** → You manage the virtual server.
* **Lambda** → You focus on the code; AWS manages the servers.

> 💡 **Remember:** ⚡ Lambda = **Run code without managing servers**

---
### Question 8 — Module 2

A company wants an application to automatically handle increases in traffic. It wants **more EC2 instances to be launched when demand increases**, and those instances should also receive incoming traffic.

**Which combination best accomplishes this?**

* **A. EC2 Auto Scaling + Elastic Load Balancing**
* B. AWS Lambda + Amazon S3
* C. Amazon RDS + Amazon DynamoDB
* D. Amazon S3 + Elastic Load Balancing

### Answer

**A. EC2 Auto Scaling + Elastic Load Balancing**

### Notes

Think of them as a team:

* **Auto Scaling** → decides **how many EC2 instances** are needed.
* **ELB** → distributes **incoming traffic** across the instances.

### Example

Normal traffic:

`Users → ELB → EC2 | EC2`

High traffic:

`Users → ELB → EC2 | EC2 | EC2 | EC2 | EC2`

> 💡 **Remember:** Auto Scaling = **How many?** | ELB = **Where does traffic go?**

---
### Question 9 — Module 2

A company hosts a web application on a single Amazon EC2 instance. During periods of high traffic, the instance becomes overloaded and the application becomes slow.

The company wants to improve the application's ability to handle increased traffic by **adding additional EC2 instances** rather than making the existing instance larger.

**Which solution should the company use?**

* A. Increase the instance size to provide more CPU and memory
* **B. Use EC2 Auto Scaling to launch additional instances based on demand**
* C. Use Amazon RDS to increase the application's compute capacity
* D. Use AWS Lambda to vertically scale the existing EC2 instance

### Answer

**B. Use EC2 Auto Scaling to launch additional instances based on demand**

### Notes

* **Adding more EC2 instances** → Horizontal scaling.
* **Automatically adding/removing instances based on demand** → EC2 Auto Scaling.
* **A** is vertical scaling because it makes the existing instance bigger.

> 💡 **Remember:** More instances = **Horizontal scaling** | Bigger instance = **Vertical scaling**


---
### Question 10 — Module 2 

A company runs a web application on several EC2 instances. During a traffic spike, the company notices that some instances receive significantly more requests than others, causing uneven resource utilization.

The company wants to distribute incoming application traffic across the available EC2 instances.

**Which AWS service should the company use?**

* A. Amazon EC2 Auto Scaling
* **B. Elastic Load Balancing (ELB)**
* C. AWS Lambda
* D. Amazon CloudWatch

### Answer

**B. Elastic Load Balancing (ELB)**

### Notes

* **ELB** → Distributes incoming traffic across EC2 instances.
* **EC2 Auto Scaling** → Adds/removes EC2 instances.
* **CloudWatch** → Monitors resources and metrics.
* **Lambda** → Runs code without managing servers.

> 💡 **Remember:** ELB = **Distribute traffic** | Auto Scaling = **Adjust instances**

---
### Question 11

A company is developing an application that needs to process an image whenever a user uploads it to Amazon S3. The image-processing code usually runs for only a few seconds.

The company does **not** want to provision or manage servers and wants to pay only when the code runs.

Which AWS service is the **best** choice?

* A. Amazon EC2
* B. Amazon ECS
* **C. AWS Lambda**
* D. Amazon RDS

### Answer

**C. AWS Lambda**

### Notes

* **Event-driven:** Runs when an image is uploaded to Amazon S3.
* **Short-running code:** Suitable for code that runs for a few seconds.
* **Serverless:** You don't provision or manage servers.
* **Pay for execution:** You pay based on Lambda usage.

### EC2 vs Lambda

```text
AWS Compute
│
├── Amazon EC2
│   └── Virtual server
│       └── You manage the server
│
└── AWS Lambda
    └── Serverless compute
        └── AWS manages the servers
```

**EC2:** You get a virtual server and manage the OS, software, and configuration.

**Lambda:** You provide your code/function, and AWS runs it when needed.

> 💡 **Remember:**
> 🖥️ **EC2 = Virtual server + more control**
> ⚡ **Lambda = Run code + no server management**

**Important:** Lambda is **not an EC2 instance**. They are separate AWS compute services.

### Event-Driven Example

```text
S3 Upload
   ↓
Lambda triggered
   ↓
Process image
   ↓
Function finishes
```

> 💡 **CLF-C02 shortcut:**
> **Event + short-running code + no server management = Lambda**

---
### Question 16

A company wants to launch a virtual server in AWS and have **full control over the operating system, installed software, and configuration**.

Which AWS service should the company use?

**A. AWS Lambda**  
**B. Amazon EC2**  
**C. Amazon ECS**  
**D. Amazon S3**

**Your answer?**

B. Amazon EC2

✅ **Correct — B. Amazon EC2**

**Why:** Amazon EC2 provides virtual servers where you have control over the **operating system, software, and configuration**.

**Exam clue:**

> “Virtual server” + “control over OS” → **Amazon EC2**

---
### Question 17 — CLF-C02 Exam Level

A company wants to run an application using containers but **does not want to manage the underlying servers or EC2 instances**.

Which AWS service is the **best choice**?

**A. Amazon EC2**  
**B. Amazon ECS with AWS Fargate**  
**C. AWS Lambda**  
**D. Amazon EC2 Auto Scaling**

**Your answer?**

B. Amazon ECS with AWS Fargate

✅ **Correct — B. Amazon ECS with AWS Fargate**

**Why:**

- **ECS** → manages and runs your **containers**
- **Fargate** → lets you run those containers **without managing EC2 servers**
- **EC2** → you manage the virtual server
- **Lambda** → runs individual functions, not general containerized applications in the usual CLF-C02 framing

**Exam clue:**

> “Containers” + “do not want to manage servers” → **Amazon ECS with AWS Fargate**

---
### Question 18 — CLF-C02 Exam Level

A company needs to run a workload that can be interrupted at any time. The workload is **not time-sensitive** and can restart from the beginning if necessary. The company wants to **minimize the cost of running EC2 instances**.

Which EC2 purchasing option is the **most cost-effective choice**?

**A. On-Demand Instances**  
**B. Reserved Instances**  
**C. Spot Instances**  
**D. Dedicated Hosts**

**Your answer?**

C. Spot Instances

✅ **Correct — C. Spot Instances**

**Why:** Spot Instances use unused EC2 capacity at a **much lower price** than On-Demand, but AWS can interrupt them when the capacity is needed.

**Exam clue:**

> **Flexible + can be interrupted + wants lowest cost → Spot Instances**

**Quick comparison:**

| Option | Best for |
|---|---|
| **On-Demand** | No long-term commitment, flexible workloads |
| **Reserved** | Predictable workloads with long-term usage |
| **Spot** ✅ | Flexible/interruption-tolerant workloads at very low cost |
| **Dedicated Host** | Dedicated physical server requirements |

### Question 19 — CLF-C02 Exam Level

A company has a workload that runs continuously and predictably for the next **3 years**. The company wants to reduce its EC2 costs compared with On-Demand pricing and is willing to make a **long-term commitment**.

Which EC2 purchasing option is the best fit?

**A. Spot Instances**  
**B. On-Demand Instances**  
**C. Reserved Instances**  
**D. Dedicated Hosts**

**Your answer?**

C. Reserved Instances

✅ **Correct — C. Reserved Instances**

**Why:** Reserved Instances are suited for **predictable, steady workloads** where you can commit to using EC2 for a longer period, typically **1 or 3 years**, in exchange for discounted pricing compared with On-Demand.

**Exam clue:**

> **Predictable workload + long-term commitment → Reserved Instances**


---
### Question 20 — CLF-C02 Exam Level

A company needs an EC2 instance for a workload that may run for only a few days. The workload is unpredictable, and the company **does not want any long-term commitment**.

Which EC2 purchasing option is the best choice?

**A. Reserved Instances**  
**B. Spot Instances**  
**C. On-Demand Instances**  
**D. Dedicated Hosts**

**Your answer?**

C. On-Demand Instances

✅ **Correct — C. On-Demand Instances**

**Why:** On-Demand is best when you need:

- No long-term commitment
- Flexible usage
- Workloads that are short-term or unpredictable

### Quick exam comparison

| EC2 option | Key clue |
|---|---|
| **On-Demand** ✅ | Short-term / unpredictable / no commitment |
| **Reserved** | Predictable / long-term commitment |
| **Spot** | Can tolerate interruption / lowest cost |
| **Dedicated Host** | Dedicated physical server |
