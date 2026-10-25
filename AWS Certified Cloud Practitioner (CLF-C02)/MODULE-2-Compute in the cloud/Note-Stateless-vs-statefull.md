# Stateful vs Stateless: Security Groups vs NACLs

CLF-C02 Question 56 study notes.

## The Question

A company wants to control inbound and outbound traffic for **individual EC2 instances** using **stateful rules**. Return traffic for an allowed connection should be permitted automatically.

**Correct answer: B. Security groups**

Key phrases to spot: "individual EC2 instances" and "stateful rules".

---

## Simple Example: The Building Guard

### Stateful: a guard with a memory

You say, "I'm here to see Priya." The guard checks the list and lets you in. When you leave, the guard remembers you came in and lets you out without checking a second list.

**One rule (allow entry) covers both directions.**

This is a **security group**.

### Stateless: a guard with no memory

The guard checks the "allowed to enter" list when you arrive. When you leave, he has no idea you were ever inside, so he checks a separate "allowed to exit" list. If your name isn't there, you're stuck.

**You need two rules: one for entering, one for leaving.**

This is a **NACL**.

---

## In AWS Terms

A user's browser sends a request to your web server on port 443.

- **Security group:** write one inbound rule allowing port 443. The response goes back automatically.
- **NACL:** write an inbound rule for port 443 *and* an outbound rule so the response can leave (including ephemeral ports).

---

## Comparison Table

| | Security Group | NACL |
|---|---|---|
| Level | Instance / resource | Subnet |
| State | Stateful | Stateless |
| Rules | Allow only | Allow and deny |
| Evaluation | All rules together | In numbered order |

---

## Why the Other Options Are Wrong

- **A. NACLs:** Stateless, applied at the subnet level.
- **C. AWS WAF:** Layer 7 filtering of web requests (SQL injection, XSS). Attaches to CloudFront, ALB, or API Gateway.
- **D. AWS Shield:** DDoS protection, not a general traffic filter.

---

## Exam Tips

- **Stateful** or **instance-level** → think **Security Group**
- **Stateless**, **subnet-level**, or **explicitly deny an IP** → think **NACL**

## Memory Trick

- **Stateful** = remembers → one rule is enough
- **Stateless** = forgets → write rules both ways