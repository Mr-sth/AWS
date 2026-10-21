# Amazon EBS: Complete Study Guide

Everything from our chat in one place: the four volume types, the extra exam topics, burst credits, and three practice quizzes with answers.

## Contents

1. [The Question](#1-the-question)
2. [The Four Volume Types (Simple Explanation)](#2-the-four-volume-types-simple-explanation)
3. [Core Concepts You Must Know](#3-core-concepts-you-must-know)
4. [Burst Credits (gp2)](#4-burst-credits-gp2)
5. [Other Exam Topics](#5-other-exam-topics)
6. [Which Type for Which Scenario](#6-which-type-for-which-scenario)
7. [Quiz 1: Basics](#7-quiz-1-basics)
8. [Quiz 2: Advanced](#8-quiz-2-advanced)
9. [Quiz 3: Full Review](#9-quiz-3-full-review)
10. [Answer Keys](#10-answer-keys)

---

## 1. The Question

**Amazon EBS volume type:**

- A. Throughput Optimized HDD (st1)
- B. Cold HDD (sc1)
- C. General Purpose SSD (gp3)
- D. Provisioned IOPS SSD (io2)

---

## 2. The Four Volume Types (Simple Explanation)

Think of EBS as a **hard drive you rent for your cloud computer (EC2)**. AWS gives you 4 main types, like 4 different kinds of storage for different jobs.

### C. General Purpose SSD (gp3): the everyday drive

- Fast, balanced, and fairly cheap.
- Good for most things: websites, small databases, dev/test servers, boot drives.
- If you're not sure what to pick, pick this one.

### D. Provisioned IOPS SSD (io2): the super-fast, premium drive

- The fastest and most reliable, but the most expensive.
- You pay to *guarantee* a certain speed (IOPS = how many read/write operations per second).
- Good for critical databases like Oracle or SQL Server, where speed must never drop.

### A. Throughput Optimized HDD (st1): the big-file highway

- An old-style spinning disk, but good at reading and writing **large amounts of data in a row**.
- Cheaper than SSD.
- Good for big data, log processing, and data warehouses.
- Not good for booting a computer.

### B. Cold HDD (sc1): the cheap storage room

- The cheapest option.
- For data you **rarely touch**, like old archives.
- Slow, so don't use it for anything active.

### Quick Memory Trick

| Type | Think of it as | Use for |
|---|---|---|
| gp3 | Normal car | Most things |
| io2 | Race car | Critical, high-speed databases |
| st1 | Big truck | Large, continuous data |
| sc1 | Parked car in storage | Rarely used data |

> **Exam tip:** Only SSD types (gp3 and io2) can be used as **boot volumes**. The HDD types (st1 and sc1) cannot.

---

## 3. Core Concepts You Must Know

### 3.1 An EBS volume lives in ONE Availability Zone

Think of an EBS volume as a drive plugged into a building (the AZ). You can't carry it to a different building. To move data to another AZ:

1. Take a **snapshot** (a backup copy, stored safely in S3).
2. Create a **new volume from that snapshot** in the other AZ.
3. Attach the new volume to your instance there.

### 3.2 Multi-Attach: one drive shared by many computers

Normally one volume connects to one EC2 instance. **Multi-Attach** lets several instances use the same volume at once.

- Only **io2 (and io1)** support it.
- All the instances must be in the **same AZ**.
- It's used for special clustered applications.

### 3.3 gp3 vs gp2 (the older version)

- With **gp2**, speed was tied to size. To get more speed, you had to buy a bigger disk, even if you didn't need the space.
- With **gp3**, you set speed (IOPS and throughput) **separately from size**. Every gp3 volume starts at 3,000 IOPS, whatever its size.
- That makes gp3 cheaper and more flexible.

### 3.4 Maximum speed limits

| Type | Max IOPS (roughly) |
|---|---|
| gp3 | 16,000 |
| io2 | 64,000 |
| io2 Block Express | 256,000 |

If a question asks for a very high number like 100,000 IOPS, gp3 is too small. The answer is **io2**.

### 3.5 Random vs. sequential data

- **Sequential** means reading one big file from start to end. HDDs (st1, sc1) are good at this.
- **Random** means jumping around, reading and writing many tiny pieces. This is what databases do, and HDDs are bad at it. Use SSD (gp3 or io2).

---

## 4. Burst Credits (gp2)

**Burst credit** is like a savings account for speed. It's how the older **gp2** volume works.

### The idea

- A gp2 volume has a **normal (baseline) speed** that depends on its size: **3 IOPS per GB**.
- When the volume is quiet, it **saves credits**, like putting money in a piggy bank.
- When it gets busy, it **spends those credits** to go faster than normal, up to **3,000 IOPS**. This is called "bursting".
- When the credits run out, speed **drops back to the baseline**.

### Example

You have a **100 GB** gp2 volume:

- Baseline speed = 100 × 3 = **300 IOPS**.
- If it's idle most of the day, it saves credits.
- When a busy period starts, it can jump up to **3,000 IOPS** while the credits last.
- After they're gone, it falls back to **300 IOPS**, and your app suddenly feels slow.

### Why this is a problem

- A small gp2 volume has a low baseline, so it depends heavily on burst credits.
- If your workload is *always* busy, it will run out of credits and slow down.
- The old fix was to buy a **bigger disk** just to get a higher baseline, even if you didn't need the space.

### How gp3 fixes it

- gp3 has **no burst credits**.
- Every gp3 volume gets a fixed **3,000 IOPS** from the start, and you can raise IOPS separately from size.
- Its speed stays the same all the time.

> **Exam tip:** If a question says an older volume "becomes slow after heavy use" or mentions "burst credits", the answer is **gp2**. The fix is to move to **gp3** (or to a larger volume or io2).

---

## 5. Other Exam Topics

### 5.1 The two families

- **SSD-backed** (gp2, gp3, io1, io2): best for small, random reads and writes. Databases, boot drives, and general use.
- **HDD-backed** (st1, sc1): best for large, continuous data. Big data and archives.
- Two older names also appear in exams: **gp2** (old general purpose) and **io1** (old Provisioned IOPS).

### 5.2 Snapshots

- A backup of a volume, stored in S3.
- **Incremental**: after the first one, only changed data is saved.
- You can restore one to any AZ, or copy it to another Region.
- Backups can be automated (Amazon Data Lifecycle Manager).

### 5.3 EBS vs Instance Store

- **EBS** is a network drive. Data stays when you stop the instance.
- **Instance Store** is a drive physically inside the host computer. It is very fast, but the **data is lost** when the instance stops or terminates. Use it only for temporary data.

### 5.4 EBS vs EFS vs S3 (a very common question)

| Service | Type | Key point |
|---|---|---|
| EBS | Block storage | One drive for one instance, inside one AZ |
| EFS | File storage | Shared by many instances across AZs (Linux) |
| S3 | Object storage | Unlimited, accessed over the internet/API, not a "drive" |

### 5.5 Data persistence

- EBS data lives **independently of the instance**.
- **Delete on Termination**: ON by default for the root (boot) volume, OFF for extra volumes.

### 5.6 Encryption

- EBS supports encryption using **AWS KMS** keys.
- If a volume is encrypted, its snapshots and volumes created from them are also encrypted.

### 5.7 Elastic Volumes

- You can change a volume's size, type, or IOPS **while it is in use**, with no downtime.
- You can **increase** size but **not decrease** it.

### 5.8 Pricing basics

- You pay for the storage you **provision** (GB per month), not what you use.
- gp3 and io2 also charge for extra IOPS.
- Snapshots cost money too.
- An unattached volume still costs money.

### 5.9 Numbers (for the Associate-level exam)

- **Durability**: io2 is 99.999%. The others are about 99.8 to 99.9%.
- **gp3**: baseline 3,000 IOPS and 125 MiB/s, up to 16,000 IOPS.
- **io2**: up to 64,000 IOPS. **io2 Block Express** goes up to 256,000 IOPS.
- **st1 and sc1**: minimum size is 125 GiB.
- **gp2**: uses a *burst credit* system, so speed can drop when credits run out.

---

## 6. Which Type for Which Scenario

| If the question says... | Pick |
|---|---|
| General use, boot drive, dev/test, "not sure" | gp3 |
| Critical database, very high or guaranteed IOPS, Multi-Attach | io2 |
| Big data, logs, data warehouse, large sequential reads, cheaper than SSD | st1 |
| Rarely accessed, lowest cost | sc1 |
| Older volume slows down after heavy use, burst credits | gp2 (move to gp3) |
| Temporary scratch data, fine to lose | Instance Store |
| Shared file system across many instances and AZs | EFS |
| Move a volume to another AZ | Snapshot, then new volume in the other AZ |

---

## 7. Quiz 1: Basics

### Question 1

You need a drive for a small website and a dev/test server. You want good speed at a fair price. Which EBS type is best?

- A. Cold HDD (sc1)
- B. General Purpose SSD (gp3)
- C. Provisioned IOPS SSD (io2)
- D. Throughput Optimized HDD (st1)

**Hint:** Think of the "normal car".

**Answer: B. gp3.** It is the everyday drive: balanced, fast enough, and cheap, so it fits most normal workloads.

---

### Question 2

A company runs a critical database. The speed must never drop, and they are happy to pay more. Which type fits?

- A. Provisioned IOPS SSD (io2)
- B. Cold HDD (sc1)
- C. General Purpose SSD (gp3)
- D. Throughput Optimized HDD (st1)

**Hint:** Think of the "race car".

**Answer: A. io2.** It lets you pay for a guaranteed level of IOPS. It is the premium choice for critical, high-speed databases.

---

### Question 3

You store old archive data that you almost never open. You want the lowest cost. Which type should you choose?

- A. General Purpose SSD (gp3)
- B. Provisioned IOPS SSD (io2)
- C. Throughput Optimized HDD (st1)
- D. Cold HDD (sc1)

**Hint:** Think of the "parked car in storage".

**Answer: D. sc1.** It is the cheapest EBS type. It is made for data you rarely access, and it is slow.

---

### Question 4

You process huge log files and big data, reading large amounts of data in a row. You want something cheaper than SSD. Which type fits best?

- A. Cold HDD (sc1)
- B. Throughput Optimized HDD (st1)
- C. Provisioned IOPS SSD (io2)
- D. General Purpose SSD (gp3)

**Hint:** Think of the "big truck".

**Answer: B. st1.** It is good at moving large amounts of data continuously, and it costs less than SSD. That suits big data and log processing.

---

### Question 5

Which of these EBS types can be used as a boot volume (the drive an EC2 instance starts from)?

- A. Throughput Optimized HDD (st1)
- B. Cold HDD (sc1)
- C. Both st1 and sc1
- D. General Purpose SSD (gp3)

**Hint:** Only SSD types can do this job.

**Answer: D. gp3.** Only SSD types (gp3 and io2) can be boot volumes. The HDD types, st1 and sc1, cannot.

---

## 8. Quiz 2: Advanced

### Question 1

An EC2 instance in us-east-1a uses an EBS volume. You need the same data on a new instance in us-east-1b. What is the correct approach?

- A. Change the volume type to io2, which works across AZs
- B. Attach the volume directly to the new instance in the other AZ
- C. Create a snapshot, then create a new volume from it in the other AZ
- D. Enable Multi-Attach on the volume

**Hint:** EBS volumes are tied to one AZ. Think about how to copy the data out and back in.

**Answer: C.** An EBS volume lives in one Availability Zone only. To move it, take a snapshot (stored in S3), then create a new volume from that snapshot in the other AZ. You cannot attach a volume across AZs.

---

### Question 2

A clustered application needs ONE EBS volume attached to several EC2 instances at the same time, all in the same AZ. Which volume type supports this (Multi-Attach)?

- A. gp3
- B. st1
- C. sc1
- D. io2

**Hint:** Only the premium Provisioned IOPS SSD family supports this feature.

**Answer: D. io2.** Multi-Attach lets one volume attach to several instances at the same time, but only for io1 and io2 volumes, and only within the same Availability Zone. gp3, st1, and sc1 do not support it.

---

### Question 3

What is a key advantage of gp3 over the older gp2?

- A. gp3 is the only type that can be encrypted
- B. gp3 lets you set IOPS and throughput independently of volume size
- C. gp3 uses spinning disks, which makes it cheaper
- D. gp3 gives unlimited IOPS on any volume

**Hint:** Think about what you can tune separately on gp3 that you could not on gp2.

**Answer: B.** With gp3, you set IOPS and throughput separately from the volume size. gp3 starts at 3,000 IOPS and 125 MiB/s no matter the size. With the older gp2, IOPS grew with size, so you often paid for extra storage just to get speed.

---

### Question 4

A single database volume must deliver 100,000 IOPS with very low latency. Which option fits?

- A. io2 (Provisioned IOPS SSD, with Block Express)
- B. gp3 with maximum settings
- C. st1
- D. sc1

**Hint:** gp3 has a maximum of 16,000 IOPS per volume.

**Answer: A.** gp3 tops out at 16,000 IOPS per volume. To go much higher, you need Provisioned IOPS SSD (io2, and io2 Block Express goes up to 256,000 IOPS). HDD types are built for throughput, not high IOPS.

---

### Question 5

A team puts a busy MySQL database (many small random reads and writes) on st1 to save money. What is the most likely result?

- A. It will run faster because st1 has the highest throughput of all types
- B. It will fail, because databases cannot use EBS
- C. It will likely perform poorly, because HDDs are bad at small random I/O
- D. It will automatically switch to SSD when needed

**Hint:** Ask what HDDs are good at: big sequential data, or lots of tiny random operations?

**Answer: C.** st1 is an HDD. It is great for large, sequential reads and writes, but weak at many small random reads and writes, which is what a busy database does. A database like this belongs on an SSD such as gp3 or io2.

---

## 9. Quiz 3: Full Review

### Question 1

You need temporary, very fast storage for scratch data. The data can be lost when the instance stops. Which storage fits?

- A. EBS gp3
- B. Instance Store
- C. EBS io2
- D. EBS snapshot

**Hint:** Which storage loses its data when the instance stops?

**Answer: B. Instance Store.** It is a drive physically inside the host computer. It is very fast, but the data is lost when the instance stops or terminates. EBS data stays after the instance stops.

---

### Question 2

Several Linux EC2 instances in different AZs need to share the same file system. Which service fits best?

- A. EBS
- B. Instance Store
- C. EFS
- D. EBS with sc1

**Hint:** Which one is shared file storage?

**Answer: C. EFS.** It is file storage that many instances can share across AZs. A normal EBS volume is block storage tied to one AZ and used by one instance (except Multi-Attach on io2, which stays in one AZ).

---

### Question 3

Which statement about EBS snapshots is TRUE?

- A. They are stored in the same AZ as the volume only
- B. Every snapshot copies the full volume again
- C. They can only be restored to the same AZ
- D. They are incremental and stored in S3

**Hint:** Think about how a snapshot saves space after the first one.

**Answer: D.** After the first snapshot, only changed data is saved (incremental). Snapshots are stored in S3 and can be restored to any AZ or copied to another Region.

---

### Question 4

You launch an EC2 instance with default settings, a root volume and one extra data volume. You then terminate the instance. What happens by default?

- A. Both volumes are deleted
- B. Both volumes are kept
- C. The root volume is deleted, and the extra volume is kept
- D. The root volume is kept, and the extra volume is deleted

**Hint:** Delete on Termination has different defaults for root and extra volumes.

**Answer: C.** Delete on Termination is ON by default for the root volume and OFF for extra volumes.

---

### Question 5

Using Elastic Volumes, which change can you NOT do?

- A. Increase the volume size
- B. Change the volume type
- C. Change the IOPS
- D. Decrease the volume size

**Hint:** Size can go in only one direction.

**Answer: D.** Elastic Volumes let you change size, type, and IOPS while the volume is in use, with no downtime. But you can only increase the size, never decrease it.

---

### Question 6

You create a snapshot of an encrypted EBS volume. What is true about the snapshot?

- A. It is encrypted too
- B. It is not encrypted
- C. It is encrypted only if you copy it to another Region
- D. It cannot be created

**Hint:** Encryption carries over to snapshots.

**Answer: A.** Snapshots of an encrypted volume are encrypted, and any volume created from them is also encrypted. EBS encryption uses AWS KMS keys.

---

### Question 7

A company deleted its EC2 instances but kept some EBS volumes detached (not attached to anything). What about the cost?

- A. Detached volumes are free
- B. They cost money only when attached
- C. They are billed only for the data actually used
- D. They still cost money, based on the size you provisioned

**Hint:** Think: provisioned or used?

**Answer: D.** You pay for the storage you provision (GB per month), not what you actually use, and this continues even when the volume is unattached.

---

### Question 8

A data warehouse reads huge files from start to end all day. The team wants lower cost than SSD, and the data is accessed often. Which type fits best?

- A. io2
- B. sc1
- C. st1
- D. gp3

**Hint:** Big, continuous data, accessed often, cheaper than SSD.

**Answer: C. st1.** It is an HDD that is good at large sequential data and is cheaper than SSD. sc1 is for rarely accessed data, so it is too slow for frequent use.

---

### Question 9

Which EBS type offers the highest durability, at 99.999%?

- A. gp3
- B. io2
- C. st1
- D. sc1

**Hint:** The premium "race car".

**Answer: B. io2.** It has 99.999% durability. The other types are about 99.8 to 99.9%.

---

### Question 10

An older gp2 volume suddenly becomes slow after heavy use. What is the most likely cause?

- A. The volume ran out of burst credits
- B. The volume was moved to another AZ
- C. The volume became an HDD
- D. The snapshot ran out of space

**Hint:** gp2 has a "credit" system.

**Answer: A.** gp2 uses a burst credit system. When the credits are used up, speed drops back to its baseline, which depends on the volume size. gp3 avoids this with a fixed baseline of 3,000 IOPS.

---

## 10. Answer Keys

### Quiz 1: Basics

| Question | Answer |
|---|---|
| 1 | B (gp3) |
| 2 | A (io2) |
| 3 | D (sc1) |
| 4 | B (st1) |
| 5 | D (gp3) |

### Quiz 2: Advanced

| Question | Answer |
|---|---|
| 1 | C (snapshot, then new volume in the other AZ) |
| 2 | D (io2) |
| 3 | B (IOPS and throughput independent of size) |
| 4 | A (io2 Block Express) |
| 5 | C (HDD is bad at small random I/O) |

### Quiz 3: Full Review

| Question | Answer |
|---|---|
| 1 | B (Instance Store) |
| 2 | C (EFS) |
| 3 | D (incremental, stored in S3) |
| 4 | C (root deleted, extra kept) |
| 5 | D (cannot decrease size) |
| 6 | A (snapshot is encrypted too) |
| 7 | D (still billed for provisioned size) |
| 8 | C (st1) |
| 9 | B (io2) |
| 10 | A (ran out of burst credits) |
