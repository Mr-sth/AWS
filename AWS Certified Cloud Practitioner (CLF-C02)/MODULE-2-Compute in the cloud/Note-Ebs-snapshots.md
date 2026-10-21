# EBS Snapshots Explained Simply

A plain-English guide to Amazon EBS snapshots, with examples. Useful for AWS Cloud Practitioner exam prep.

## The basic idea

An **EBS snapshot** is a **backup of your EBS volume at a specific moment in time**.

Think of it as taking a **photo of your hard drive**. Later, you can use that photo to bring the drive back exactly as it looked.

## Simple real-life example

Imagine you're writing a big report on your laptop.

- Every evening you **save a copy** of the report in a safe place
- On Wednesday your laptop dies and the file is corrupted
- You go back to Tuesday's copy and carry on from there

An EBS snapshot is that saved copy for your cloud disk.

## Key features (in plain words)

1. **Point-in-time backup:** it captures the volume as it was when you took the snapshot.
2. **Stored in S3 (managed by AWS):** the data is kept durably across multiple AZs. You won't see it in your own S3 buckets, though. You manage it from the EBS console.
3. **Incremental:** the first snapshot copies everything. Each later snapshot saves **only the blocks that changed** since the last one, which saves time and money.
4. **Regional, not tied to one AZ:** a snapshot belongs to a Region. You can use it to create a new volume in **any AZ in that Region**. This is how you move an EBS volume between AZs.
5. **Copyable:** you can copy a snapshot to **another Region** (for disaster recovery) or share it with **another AWS account**.
6. **Encryption carries over:** a snapshot of an encrypted volume is encrypted, and volumes made from it are encrypted too.
7. **Taken while the volume is in use:** you don't have to shut down the instance. For the cleanest backup of a busy database, though, pause writes or stop the instance first, because a snapshot captures only data already written to disk.

## How incremental snapshots work

Say you have a 100 GB volume:

| Snapshot | What changed | What gets stored |
|----------|--------------|------------------|
| Snapshot 1 (Monday) | Everything is new | About 100 GB (full copy) |
| Snapshot 2 (Tuesday) | 5 GB of data changed | About 5 GB (only the changes) |
| Snapshot 3 (Wednesday) | 2 GB of data changed | About 2 GB (only the changes) |

Each snapshot still works as a **complete backup**. AWS tracks which blocks belong to which snapshot. If you delete Snapshot 1, AWS keeps the blocks that later snapshots still need, so Snapshots 2 and 3 are not broken.

## AWS example: recovering an online shop

Say your shop's database lives on a 200 GB EBS volume.

1. Every night at 2 AM, a snapshot is taken automatically
2. One morning, a bad software update corrupts the database
3. You create a **new volume from last night's snapshot**
4. You attach it to your EC2 instance, and your shop is back, with at most one day of lost data

## Moving data across AZs and Regions

Since EBS volumes are stuck in one AZ, snapshots are the bridge:

- **To a different AZ:** snapshot the volume, then create a new volume from it in the other AZ
- **To a different Region:** snapshot the volume, **copy the snapshot** to the other Region, then create a volume there

## Automating snapshots

Taking snapshots by hand is easy to forget. AWS offers:

- **Amazon Data Lifecycle Manager (DLM):** set a schedule (for example, daily, keep the last 7). It creates and deletes snapshots for you.
- **AWS Backup:** a central service that manages backups for many AWS services, including EBS.

## Other features worth knowing

- **Snapshot Archive tier:** a much cheaper tier for snapshots you rarely need (about 75% cheaper). It has a 90-day minimum, and restoring takes hours.
- **Recycle Bin:** if you delete a snapshot by mistake, you can recover it within a retention period you set.
- **Fast Snapshot Restore (FSR):** by default, a volume made from a snapshot loads data lazily, so it can be slow at first. FSR removes that delay, for an extra fee.
- **Create an AMI:** you can build an Amazon Machine Image from a snapshot of a root volume, then launch new instances from it.

## Cost

You pay for the **amount of snapshot data stored** (per GB per month). Because snapshots are incremental, the cost is based on changed data, not on the full volume size for every snapshot.

## EBS snapshots vs. other things you've learned

| | **EBS snapshot** | **S3 lifecycle policy** | **EFS** |
|---|------------------|-------------------------|---------|
| What it is | A backup of an EBS volume | Rules to move or delete S3 objects | A shared file system |
| Purpose | Recovery and copying | Cost savings | Shared storage |

## Exam tips

- "**Back up an EBS volume**" points to a **snapshot**.
- "**Move an EBS volume to another AZ**" means snapshot, then create a new volume in the target AZ.
- "**Move to another Region**" means snapshot, **copy** the snapshot, then restore.
- "**Automate snapshot creation and retention**" points to **Amazon Data Lifecycle Manager**.
- Snapshots are **incremental** and stored in **S3** (managed by AWS).
- Snapshots are **Region-level**, while the volume itself is **AZ-level**.

## One-line summary

> **An EBS snapshot is an incremental, point-in-time backup of your EBS volume, stored safely in S3, that you can use to restore the disk or copy it to another AZ or Region.**
