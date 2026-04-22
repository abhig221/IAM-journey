# Principle of Least Privilege

## Overview
Least privilege is the principle that users, systems, and processes should only have access to what they actually need to do their job — nothing more. Every extra permission granted is an additional attack surface. If an account gets compromised, least privilege limits how much damage can actually be done.

---

## Standing access vs just-in-time access

**Standing access** means a user has permanent permissions whether they're actively using them or not. Convenient, but dangerous — those credentials are a target every second they exist.

**Just-in-time (JIT) access** means permissions are granted only when needed and automatically revoked when the task is done. So instead of a sysadmin having Domain Admin rights 24/7, they request them for 2 hours, complete the task, and the rights are gone. If their account gets compromised outside that window, the attacker gets nothing useful.

---

## Over-provisioning

Over-provisioning is giving someone more access than their role actually requires. It happens gradually — someone gets promoted and keeps all their old access, someone switches teams and the old permissions never get cleaned up. Over time you end up with users who have way more access than they should.

This is the number one IAM risk because it's invisible until something goes wrong. Access reviews exist specifically to catch and fix this.

---

## Connection to PAM

Privileged Access Management is least privilege applied to the most dangerous accounts — admins, service accounts, root accounts. A compromised regular user account is bad. A compromised Domain Admin is catastrophic. PAM tools like CyberArk enforce least privilege on these accounts through vaulting, session recording, and just-in-time provisioning.

---

## Real-world example: privilege escalation in OpenClaw

OpenClaw is an open-source autonomous AI agent that runs locally and performs real-world tasks — managing emails, controlling files, checking flight status — by connecting AI models like GPT or Claude to external tools through apps like Telegram and WhatsApp.

It uses a tiered permission model to control what users can instruct the agent to do. The lowest tier is basic pairing access.

### The vulnerability

The `/pair approve` command, which grants admin access, doesn't check who is actually issuing the approval. A user with basic pairing access — the lowest tier — can run `/pair approve` and self-approve themselves straight to admin. No authorization check at all.

### What IAM principles this violates

**Least privilege** — the approve command shouldn't be accessible at the lowest permission tier. The gate that should restrict this to an authorized role is completely missing.

**Separation of duties** — the person requesting elevated access and the person approving it are the same person. That's the most basic SoD violation there is. These two actions need to be separated.

**Missing authorization check** — the system authenticates who you are but never checks whether you're actually allowed to do what you're trying to do.

**Standing access** — once someone self-approves to admin, those permissions stay permanently. No time limit, no auto-revocation, no scope restriction.

### Why it's worse in an agentic system

In a normal app, privilege escalation means you can read more data. In an autonomous agent like OpenClaw, admin access means you can instruct a process that takes real-world actions — sending emails, accessing files, making API calls. The blast radius is completely different.

### What the correct controls look like

- Only users in a designated approver role can execute `/pair approve`
- Self-approval is rejected — requestor and approver must be different entities
- Elevated permissions are time-limited and automatically revoked
- Every approval is logged with who approved, when, and what was granted

### The bigger picture

As AI agents get integrated into more enterprise workflows, the IAM question of what permissions an autonomous process should hold — and who can elevate them — becomes critical. OpenClaw is a real example of what happens when those questions aren't answered at the design level. Least privilege doesn't just apply to human users anymore.

---

## Questions to revisit
- [ ] How does PIM enforce JIT access technically in Entra ID?
- [ ] What is the difference between privilege escalation and lateral movement?
- [ ] How do access reviews detect over-provisioning in practice?
