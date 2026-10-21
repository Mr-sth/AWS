# Amazon EBS Volume Types: Simple Notes

## The Question

**Amazon EBS volume type:**

- A. Throughput Optimized HDD (st1)
- B. Cold HDD (sc1)
- C. General Purpose SSD (gp3)
- D. Provisioned IOPS SSD (io2)

---

## Simple Explanation

Think of EBS as a **hard drive you rent for your cloud computer (EC2)**. AWS gives you 4 types, like 4 different kinds of storage for different jobs.

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

---

## Quick Memory Trick

| Type | Think of it as | Use for |
|---|---|---|
| gp3 | Normal car | Most things |
| io2 | Race car | Critical, high-speed databases |
| st1 | Big truck | Large, continuous data |
| sc1 | Parked car in storage | Rarely used data |

> **Exam tip:** Only SSD types (gp3 and io2) can be used as **boot volumes**. The HDD types (st1 and sc1) cannot.

---

## Practice Quiz: EBS Volume Types

*Practice choosing the right EBS volume type for each job.*

### Question 1

You need a drive for a small website and a dev/test server. You want good speed at a fair price. Which EBS type is best?

- A. Cold HDD (sc1)
- B. General Purpose SSD (gp3)
- C. Provisioned IOPS SSD (io2)
- D. Throughput Optimized HDD (st1)

**Hint:** Think of the "normal car".

**Answer: B. General Purpose SSD (gp3)**

gp3 is the everyday drive. It is balanced, fast enough, and cheap, so it fits most normal workloads.

---

### Question 2

A company runs a critical database. The speed must never drop, and they are happy to pay more. Which type fits?

- A. Provisioned IOPS SSD (io2)
- B. Cold HDD (sc1)
- C. General Purpose SSD (gp3)
- D. Throughput Optimized HDD (st1)

**Hint:** Think of the "race car".

**Answer: A. Provisioned IOPS SSD (io2)**

io2 lets you pay for a guaranteed level of IOPS. It is the premium choice for critical, high-speed databases.

---

### Question 3

You store old archive data that you almost never open. You want the lowest cost. Which type should you choose?

- A. General Purpose SSD (gp3)
- B. Provisioned IOPS SSD (io2)
- C. Throughput Optimized HDD (st1)
- D. Cold HDD (sc1)

**Hint:** Think of the "parked car in storage".

**Answer: D. Cold HDD (sc1)**

sc1 is the cheapest EBS type. It is made for data you rarely access, and it is slow.

---

### Question 4

You process huge log files and big data, reading large amounts of data in a row. You want something cheaper than SSD. Which type fits best?

- A. Cold HDD (sc1)
- B. Throughput Optimized HDD (st1)
- C. Provisioned IOPS SSD (io2)
- D. General Purpose SSD (gp3)

**Hint:** Think of the "big truck".

**Answer: B. Throughput Optimized HDD (st1)**

st1 is good at moving large amounts of data continuously, and it costs less than SSD. That suits big data and log processing.

---

### Question 5

Which of these EBS types can be used as a boot volume (the drive an EC2 instance starts from)?

- A. Throughput Optimized HDD (st1)
- B. Cold HDD (sc1)
- C. Both st1 and sc1
- D. General Purpose SSD (gp3)

**Hint:** Only SSD types can do this job.

**Answer: D. General Purpose SSD (gp3)**

Only SSD types (gp3 and io2) can be boot volumes. The HDD types, st1 and sc1, cannot.

---

## Answer Key

| Question | Answer |
|---|---|
| 1 | B (gp3) |
| 2 | A (io2) |
| 3 | D (sc1) |
| 4 | B (st1) |
| 5 | D (gp3) |
