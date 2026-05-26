---
title: "5 Penetration Test Findings Companies Keep Ignoring (And Why They Shouldn't)"
description: "After hundreds of engagements, we keep seeing the same critical findings get deprioritized. Here's what they are and what the real-world impact looks like."
pubDate: 2026-05-15
author: "Sable Fox"
tags: ["Penetration Testing", "Red Team"]
draft: false
---

After running hundreds of penetration tests across finance, healthcare, and SaaS companies, we've noticed a pattern: the same high-severity findings keep appearing on remediation backlogs — sometimes for *years*.

This isn't about negligence. It's usually about prioritization pressure, unclear ownership, or a genuine misunderstanding of the blast radius. This post breaks down the five we see most often and what actually happens when they get exploited.

## 1. Kerberoastable Service Accounts with Weak Passwords

Kerberoasting lets any authenticated domain user request a Kerberos service ticket for an account running a service — and take that ticket offline to crack. If the account has a weak password and high privileges, this is a direct path to domain compromise.

We find at least one kerberoastable account in the majority of Active Directory environments we test. The fix is simple: use strong, randomly generated passwords (25+ characters) for service accounts, or better yet, use Group Managed Service Accounts (gMSAs).

**Why it gets ignored:** "We'll rotate passwords next quarter." Next quarter becomes next year.

## 2. Overly Permissive AWS IAM Roles

The cloud IAM problem isn't just `AdministratorAccess` attached to a Lambda — it's subtler than that. We routinely find roles with `sts:AssumeRole` permissions that allow privilege escalation paths not immediately obvious from a single policy document.

A misconfigured trust policy can let an attacker with access to a low-privilege EC2 instance pivot to a role with `s3:GetObject` on every bucket in the account.

**Why it gets ignored:** IAM is complex, the blast radius isn't always visible, and nobody wants to break a production service.

## 3. Unauthenticated Internal Services Exposed via VPN Split Tunneling

VPN split tunneling is convenient. It's also frequently misconfigured in ways that expose internal services — monitoring dashboards, build systems, internal APIs — to traffic that was never meant to reach them.

We've accessed Grafana dashboards, Jenkins instances, and internal Swagger docs through this vector. From there, it's usually a short walk to credentials or code execution.

**Why it gets ignored:** "It's internal." Internal doesn't mean trusted.

## 4. Verbose Error Messages and Stack Traces in Production

This one looks low-severity on paper. In practice, error messages that include stack traces, framework versions, database query details, or internal IP addresses give attackers a significant reconnaissance advantage.

Combined with a known CVE in an exposed framework version, this transforms from "informational" to "critical" very quickly.

**Why it gets ignored:** Development teams own the fix, but security teams own the finding — and the handoff is messy.

## 5. Legacy Authentication Protocols Still Enabled

NTLM, NTLMv1, basic auth over unencrypted channels — these keep showing up. Legacy authentication bypasses modern conditional access policies in Azure AD and is the first thing we target in phishing simulations.

Disabling legacy auth is a forcing function that sometimes breaks old integrations, which is why it stays on the backlog.

**Why it gets ignored:** Fear of breaking something. We get it. But the risk of keeping it on outweighs the discomfort of the migration.

---

## The Pattern

Every one of these findings shares the same lifecycle: discovered, documented, deprioritized. The common thread is that remediation requires coordination across teams — security, engineering, and ops — and nobody owns the outcome end-to-end.

Our remediation SLA data shows that findings with a clear, single owner get fixed 3x faster than those shared across teams. If you take one thing from this post, it's that: assign an owner, not a team.

---

*Want a second opinion on your current backlog? [Get in touch](/contact) — we offer free 30-minute remediation prioritization calls for companies that have completed a test in the last 12 months.*
