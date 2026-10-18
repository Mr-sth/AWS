# Amazon EBS Explained Simply

A plain-English guide to Amazon Elastic Block Store (EBS), with examples. Useful for AWS Cloud Practitioner exam prep.

## The basic idea

Think of **EBS** as a **hard drive that you plug into your cloud computer**.

In AWS, your virtual computer is called an **EC2 instance**. By itself, an EC2 instance needs somewhere to store files, like your operating system, programs, and data. That "somewhere" is often **EBS**.

- **EBS = a virtual hard disk in the cloud**
- It attaches to an EC2 instance over the network
- Your computer sees it like a normal disk (like a C: drive or an external USB drive)

## Simple real-life example

Imagine you have a **laptop** and an **external hard drive**.

- The laptop = **EC2 instance**
- The external hard drive = **EBS volume**

Now:

- If you turn off your laptop, the files on the external drive are **still safe**
- If your laptop breaks, you can **unplug the drive and plug it into another laptop**, and all your data is still there

EBS works the same way. If your EC2 instance stops or crashes, your data on EBS stays safe, and you can attach the volume to a different instance.

## Key features (in plain words)

1. **Persistent storage:** Data stays even when the instance is stopped. (By default, the root volume is deleted if you *terminate* the instance, but you can change that setting.)
2. **Attached to one instance at a time** (for most volume types): It's like a personal disk, not a shared folder.
3. **Same Availability Zone only:** The volume and the instance must be in the same AZ (data center area). You can't attach a volume from one zone to an instance in another, though you can copy it using a snapshot.
4. **Snapshots (backups):** You can take a "photo" of your disk at any time and store it safely in S3. Later, you can restore it or create a new volume from it.
5. **Resizable:** You can increase the size or change the type later without losing data.

## Types of EBS volumes (simple version)

| Type | Think of it as | Use for |
|------|----------------|---------|
| **gp3 / gp2** (General Purpose SSD) | Everyday disk | Most apps, websites, small databases |
| **io1 / io2** (Provisioned IOPS SSD) | Super-fast disk | Heavy databases that need high speed |
| **st1** (Throughput HDD) | Big, cheap disk for large files | Big data, log processing |
| **sc1** (Cold HDD) | Cheapest, slowest | Data you rarely access |

## Example scenario

Say you run a **small online shop** on AWS:

1. You launch an **EC2 instance** to run your website
2. You attach a **100 GB gp3 EBS volume** to store your database and product images
3. Every night, you take an **EBS snapshot** as a backup
4. One day your server crashes. You launch a new EC2 instance, create a volume from yesterday's snapshot, and attach it. Your shop is back online with your data.

## EBS vs. other AWS storage (common exam question)

- **EBS:** a disk for one EC2 instance (block storage)
- **S3:** stores files/objects like photos and backups, accessed over the internet (object storage)
- **EFS:** a shared folder that many instances can use at once (file storage)

## One-line summary

> **EBS is a cloud hard drive for your EC2 instance. It keeps your data safe, can be backed up with snapshots, and can be moved between instances.**
