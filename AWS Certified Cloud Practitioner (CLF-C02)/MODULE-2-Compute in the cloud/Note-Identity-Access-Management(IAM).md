# AWS IAM Identity Center
 
## What is it?
 
IAM Identity Center is the AWS service that lets people **log in once and access many AWS accounts and apps**. It used to be called **AWS Single Sign-On (SSO)**.
 
Think of it like a **company ID card**. You swipe it at the main gate once, and the card decides which rooms (accounts) you can enter and what you can do inside.
 
## The problem it solves
 
Imagine a company with 10 AWS accounts: Dev, Test, Production, Finance, and so on. Without Identity Center:
 
- You create a separate IAM user in each account.
- A person who needs 4 accounts has 4 usernames and 4 passwords.
- When someone leaves the company, you must delete them from every account. Miss one, and that is a security risk.
With Identity Center:
 
- You create the user **once**.
- You give them access to the accounts they need.
- When they leave, you disable them in **one place**.
## Key parts (simple definitions)
 
| Term | Meaning | Example |
|---|---|---|
| **Identity source** | Where your users are stored | Identity Center's own directory, Active Directory, or Okta/Azure AD |
| **User / Group** | A person, or a team of people | User: Ram. Group: "Data-Engineers" |
| **Permission set** | A template of permissions (like an IAM policy bundle) | "ReadOnlyAccess" or "AdminAccess" |
| **AWS account assignment** | Connects a user or group + permission set + account | Data-Engineers get ReadOnly on the Production account |
| **AWS access portal** | The web page where users log in and pick an account | `https://yourcompany.awsapps.com/start` |
 
## A simple example
 
Company: **NepalTech Pvt. Ltd.** with two accounts, **Dev** and **Prod**.
 
1. Create a group called **Data-Engineers** and add Sita and Ram.
2. Create two permission sets:
   - **DevAdmin**: full access
   - **ProdReadOnly**: view only
3. Assign:
   - Data-Engineers + DevAdmin → **Dev account**
   - Data-Engineers + ProdReadOnly → **Prod account**
**What Sita experiences:**
She opens the portal, logs in once, and sees two tiles: Dev and Prod. In Dev she can create and delete resources. In Prod she can only look. No extra passwords.
 
When Sita moves to another team, you remove her from the group and her access to both accounts disappears.
 
## How it works behind the scenes
 
When Sita clicks "Dev", Identity Center gives her **temporary credentials** for that account. These expire automatically, which is safer than long-lived access keys.
 
## Why it is better than plain IAM users
 
- **One login** for many accounts
- **Central control** of who can do what
- **Temporary credentials**, so fewer leaked keys
- Works with **AWS Organizations** to manage many accounts
- Can connect to your **existing company directory** (like Microsoft Active Directory)
- Supports **MFA** (multi-factor authentication)
- It is **free** to use
## IAM vs IAM Identity Center
 
| | IAM | IAM Identity Center |
|---|---|---|
| Best for | One account, applications, services | Many accounts, human users |
| Users | Created in each account | Created once, centrally |
| Credentials | Can be long-lived | Temporary |
| Typical use | Roles for EC2/Lambda, small setups | Workforce (employees) access |
 
## Exam tip (Cloud Practitioner)
 
If the question says **"manage workforce access to multiple AWS accounts with single sign-on"**, the answer is **IAM Identity Center**.
 
## Quick summary
 
IAM Identity Center = **one login, many accounts, central control, temporary access.**