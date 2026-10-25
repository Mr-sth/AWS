# AWS Security Services: Simple Notes

## 1. Amazon GuardDuty: The Security Guard

Watches your AWS account activity and network traffic and warns you when something looks suspicious. It uses machine learning, so you don't write rules.

**Example:** Someone in another country logs in with your credentials and starts launching servers. GuardDuty alerts: "This looks like a hacked account."

**Exam keywords:** malicious activity, threat detection

---

## 2. AWS IAM Access Analyzer: The Access Checker

Looks at your permissions and finds resources shared with someone outside your account or organization.

**Example:** You accidentally made an S3 bucket accessible to an outside AWS account. Access Analyzer flags it: "This bucket is shared externally."

**Exam keywords:** external access, unintended sharing

---

## 3. AWS CloudTrail: The CCTV and Logbook

Records who did what in your AWS account: which user, which action, when, and from where.

**Example:** Someone deleted an EC2 instance. CloudTrail shows it was user "Ram" at 3:15 PM from a given IP address.

**Exam keywords:** who did what, audit, API calls

---

## 4. AWS Secrets Manager: The Password Safe

Stores and protects secrets like database passwords and API keys, and can rotate them automatically. Your code fetches the secret when needed instead of hardcoding it.

**Example:** Your app asks Secrets Manager for the database password at runtime, so the password is never written in your code files.

**Exam keywords:** store credentials, rotate credentials

---

## 5. AWS WAF (Web Application Firewall): The Bouncer for Your Website

Sits in front of your web application and checks every incoming web request. You set rules, and it blocks or allows requests. It protects against common web attacks like SQL injection and cross-site scripting (XSS). It can also block specific IP addresses, countries, or too many requests from one source.

**Example 1:** A hacker types malicious code into your login form (SQL injection). WAF sees the harmful pattern and blocks it before it reaches your app.

**Example 2:** A bot sends 10,000 requests per minute. A rate-based rule blocks that IP automatically.

**Works with:** Amazon CloudFront, Application Load Balancer (ALB), API Gateway, AWS AppSync

**Exam keywords:** SQL injection, cross-site scripting, block web requests, block IP addresses or countries

### Don't confuse WAF with

- **AWS Shield:** protects against DDoS attacks. WAF protects against application-level attacks.
- **Security Groups / NACLs:** control network traffic by IP and port, but can't read web request content.

> **Quick trick:** WAF = web request filter. Shield = DDoS protection.

---

## Quick Comparison

| Service | Think of it as | Job | Keyword in exam questions |
|---|---|---|---|
| **GuardDuty** | Threat detector | Detects threats, doesn't block | malicious activity, threat detection |
| **IAM Access Analyzer** | Access checker | Finds unintended external access | external access, unintended sharing |
| **CloudTrail** | Activity logbook | Logs who did what | who did what, audit, API calls |
| **Secrets Manager** | Password vault | Stores and rotates secrets | store/rotate credentials |
| **AWS WAF** | Web bouncer | Blocks bad web requests (Layer 7) | SQL injection, XSS, block IPs |