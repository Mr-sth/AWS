# AWS Transit Gateway

```
        ┌─────────┐      ┌─────────┐      ┌─────────┐
        │  VPC A  │      │  VPC B  │      │  VPC C  │
        │   Web   │      │   DB    │      │Analytics│
        └────┬────┘      └────┬────┘      └────┬────┘
             │                │                │
             └────────┐       │       ┌────────┘
                      ▼       ▼       ▼
                ┌───────────────────────────┐
                │      TRANSIT GATEWAY      │
                │        (central hub)      │
                └─────────────┬─────────────┘
                              │
                              ▼
                ┌───────────────────────────┐
                │    On-premises network    │
                │   (VPN or Direct Connect) │
                └───────────────────────────┘
```

Every network makes **one connection** to the hub, and the hub routes traffic between all of them.

---

## 1. What is it?

AWS Transit Gateway is a **central hub** that connects many VPCs and on-premises networks through a single gateway. It follows a **hub-and-spoke** model.

**Real-life example:** An airport hub. Instead of running a direct flight between every pair of cities, airlines fly everything through one big hub. City A → Hub → City B.

---

## 2. The problem it solves

Say you have 4 networks (3 VPCs plus your office) and want them all to talk to each other.

| Approach | 4 networks | 10 networks |
|---|---|---|
| Without Transit Gateway (full mesh of peering/VPNs) | 6 connections | 45 connections |
| With Transit Gateway | 4 connections | 10 connections |

Adding a new VPC later = **one new attachment**, not a new link to every other network.

---

## 3. Key concepts

| Term | Simple meaning |
|---|---|
| **Transit Gateway (TGW)** | The central hub/router |
| **Attachment** | The link between the TGW and a network (VPC, VPN, Direct Connect gateway, or another TGW) |
| **TGW route table** | Rules that decide which attachments can talk to which |
| **Association** | Links an attachment to a route table |
| **Propagation** | Lets an attachment automatically add its routes to a route table |
| **TGW peering** | Connects Transit Gateways across Regions |

---

## 4. What can attach to a Transit Gateway?

- VPCs
- Site-to-Site VPN connections
- Direct Connect gateways
- Other Transit Gateways (peering, including cross-Region)

---

## 5. Isolating networks with route tables

By default you can let all attachments talk to each other, but separate TGW route tables let you control this.

Example:
- Dev VPC and Prod VPC are both attached to the same TGW.
- Use different route tables so **Dev cannot reach Prod**, while both can still reach a shared services VPC.

---

## 6. Example scenario

A company has:
- VPC A (web servers), VPC B (databases), VPC C (analytics)
- An office connected by VPN or Direct Connect

They attach all four to one Transit Gateway. Now:
- Web servers reach databases
- Analytics reads from databases
- Office staff reach everything

If the company adds VPC D next month, it is one new attachment instead of four new peering connections.

---

## 7. Transit Gateway vs VPC Peering

| | VPC Peering | Transit Gateway |
|---|---|---|
| Layout | One-to-one links | Hub and spoke |
| Scales to many VPCs? | Poorly (mesh gets messy) | Yes |
| Transitive routing? | **No** (A↔B and B↔C does not mean A↔C) | **Yes** |
| Connects on-premises? | No | Yes (VPN, Direct Connect) |
| Cost | Cheaper for 2 VPCs | Hourly attachment charge + per-GB data charge |

---

## 8. Related services (quick recap)

| Service | Simple meaning |
|---|---|
| VPC | Your private network in AWS |
| VPC Peering | Private link between 2 VPCs |
| Site-to-Site VPN | Encrypted tunnel over the internet |
| Direct Connect | Dedicated private line to AWS (not encrypted by default) |
| Transit Gateway | Hub connecting many VPCs and on-premises networks |

---

## 9. Exam tips (Cloud Practitioner)

- Transit Gateway = **central hub** connecting many VPCs and on-premises networks.
- Keywords to spot: "hub-and-spoke", "simplify network", "many VPCs", "reduce peering connections".
- VPC Peering is **not transitive**; Transit Gateway **is**.
- For just 2 VPCs, peering is simpler and cheaper. For many networks, choose Transit Gateway.
- A Transit Gateway is **regional**, but you can peer TGWs across Regions.

---

## 10. One-line summary

> Transit Gateway turns a messy web of network connections into a clean hub-and-spoke: one link per network, one place to manage routing.