# Amazon EFS (Elastic File System) Explained Simply

A plain-English guide to Amazon EFS, with examples. Useful for AWS Cloud Practitioner exam prep.

## The basic idea

**Amazon EFS** is a **shared network folder in the cloud**. Many servers can read and write the same files at the same time.

Compare it with EBS. EBS is a hard drive plugged into **one** computer. EFS is a **shared drive that many computers use together**.

## Simple real-life example

Think of a **shared folder in your office** (like a team drive).

- Everyone on the team opens the same folder
- If one person saves a file, everyone else sees it right away
- The folder doesn't have a fixed size. It grows as you add files and shrinks as you delete them

EFS works the same way, except the "team" is your EC2 instances, containers, and Lambda functions.

## Key features (in plain words)

1. **Shared access:** thousands of instances can connect at once, and they all see the same files.
2. **Elastic:** it grows and shrinks automatically. You never choose a size, and you pay only for what you store.
3. **Multi-AZ by default:** data is stored across multiple Availability Zones, so instances in different AZs can all use it. This is different from EBS, which is tied to one AZ.
4. **Linux only:** it uses the **NFS** protocol, so it works with Linux instances, not Windows. (For Windows file sharing, AWS has a different service called FSx for Windows File Server.)
5. **Fully managed:** AWS handles the hardware, scaling, and backups. You just mount it.
6. **Secure:** supports encryption at rest and in transit, and access control through security groups and IAM.
7. **Reachable from on-premises:** you can connect to it from your own data center over Direct Connect or VPN.

## How you use it

You **mount** EFS onto your instance, like plugging in a network drive. After that, it looks like a normal folder (for example `/mnt/shared`). Your apps read and write files there as usual, with no special code needed.

## AWS example: a WordPress website on many servers

Say you run a busy blog on **5 EC2 instances** behind a load balancer.

- Visitors upload images, and the upload lands on one of the 5 servers
- With **EBS**, that image would exist only on that one server, so other servers couldn't show it
- With **EFS**, all 5 servers mount the same file system, so every server sees every image instantly

If traffic spikes and you add 10 more servers, they just mount the same EFS and get all the files.

## Storage classes (to cut costs)

EFS has storage classes, similar to S3:

| Class | Think of it as | Use for |
|-------|----------------|---------|
| **Standard** | Everyday storage | Files used often |
| **Infrequent Access (IA)** | Cheaper, with a retrieval fee | Files rarely used |
| **Archive** | Cheapest | Files almost never used |
| **One Zone / One Zone-IA** | Same as above, but stored in a single AZ | Cheaper, for data that doesn't need multi-AZ protection |

You can turn on **lifecycle management**, which automatically moves files you haven't touched for a set number of days (for example 30) into the cheaper class. It works like S3 lifecycle policies.

## EFS vs. EBS vs. S3

| | **EBS** | **EFS** | **S3** |
|---|---------|---------|--------|
| Type | Block storage | File storage | Object storage |
| Think of it as | A hard drive in one computer | A shared network folder | A giant online storage bucket |
| Used by | **One** instance (mostly) | **Many** instances at once | Anyone over the internet or API |
| Availability Zones | One AZ | Multiple AZs (or one, if you choose) | Multiple AZs |
| Size | You choose it | Grows and shrinks automatically | Unlimited |
| Operating system | Linux and Windows | **Linux only** | Any (it's an API) |

## When to use EFS

- Several servers need to **share the same files**
- Content management systems, web serving, shared code or home directories
- Big data and machine learning workloads that read shared datasets
- You don't want to guess how much storage you'll need

## When NOT to use it

- A single server with one disk: **EBS** is simpler and cheaper
- Windows workloads: use **FSx for Windows File Server**
- Storing photos, backups, or static website files: **S3** is usually cheaper

## Exam tips

- "**Shared file system** for **multiple EC2 instances**" points to **EFS**.
- "**Linux**" plus "**NFS**" points to **EFS**.
- "**Automatically scales**, no capacity planning" points to **EFS**.
- "**Windows** file share" does **not** point to EFS. It points to FSx for Windows File Server.

## One-line summary

> **EFS is a shared, automatically growing network folder for Linux that many EC2 instances can use at the same time, across multiple Availability Zones.**
