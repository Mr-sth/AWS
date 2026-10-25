# AWS Services 

## SNS: the loudspeaker 📢

SNS sends **one message to many people at the same time**.

You send a message to a **topic**. Everyone who has subscribed to that topic gets a copy right away. Subscribers can be email, text messages, phone notifications, or other AWS services.

**Example:** A customer places an order. The store sends one message, and SNS delivers it to the email system, the analytics system, and the warehouse alert system.

**Key point:** SNS does **not keep** messages. It delivers them and moves on.

---

## SQS: the mailbox 📬

SQS **holds messages in a line (queue)** until a worker is ready to handle them.

The sender puts messages in the queue. The receiver picks them up when it has time. If the receiver is busy or broken, the messages just wait safely.

**Example:** During a big sale, thousands of orders come in. They wait in the queue, and the payment system processes them one by one at its own speed. No order is lost.

**Key point:** SQS **keeps** messages until they are processed.

---

## EventBridge: the traffic controller 🚦

EventBridge **watches for events** (things that happen) and **sends each one to the right place** using rules.

A rule says: "If *this* happens, do *that*."

**Example:** An EC2 server stops. EventBridge sees the event, matches your rule, and triggers a notification or another action.

**Key point:** It decides **where an event should go** based on rules. It can also run tasks on a schedule, like every night at midnight.

---

## Step Functions: the project manager 📋

Step Functions **runs a list of steps in the right order**. It also handles problems.

It can:
- Do steps one after another
- Choose a path (if this, then that)
- Retry a step that failed
- Run a backup step when something goes wrong

**Example:** An order workflow checks stock, takes payment, arranges shipping, and cancels the order if payment fails.

**Key point:** It **coordinates a multi-step process**.

---

## Quick comparison

| Service | Simple idea | Remember |
|---|---|---|
| **SNS** | Send one message to many | Loudspeaker |
| **SQS** | Store messages until processed | Mailbox |
| **EventBridge** | Send events to targets using rules | Traffic controller |
| **Step Functions** | Run steps in order | Project manager |

---

## Exam shortcut

- Messages **wait** until processed → **SQS**
- **One message to many** subscribers → **SNS**
- **Rules** decide where events go → **EventBridge**
- **Steps in order** with retries and errors → **Step Functions**

---

## Using SNS and SQS together

They work well as a pair. SNS sends one message to many **queues**, and each queue holds the message for its own service.

**Example:** The order service sends one message to SNS. SNS copies it into three SQS queues (payment, analytics, shipping). Each service reads from its queue when it is ready. If one service is down, its messages wait safely, and the others keep working.