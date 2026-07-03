# Kerberos

---

## Overview

Kerberos is an authentication protocol used by Active Directory to verify
the identity of users and services. It uses the KDC, or Key Distribution
Center, to authenticate users based on passwords. The KDC contains two
main components — the Authentication Service (AS) and the Ticket Granting
Service (TGS).

---

## How Kerberos works

When a user logs in, their password is hashed and sent to the AS as part
of an authentication request called the AS-REQ. The AS checks that
password hash against what is stored in the Active Directory database. If
they match, the AS-REP is returned to the user containing two things — a
TGT (Ticket Granting Ticket) and a session key.

The session key is an encryption key used to secure communication between
the client and the KDC so that subsequent requests can be verified. The
TGT is proof that the user has already authenticated and does not need to
enter their password again.

This is what makes Kerberos a form of SSO, or Single Sign-On. The user
logs in with their password once and from that point the TGT handles
authentication automatically for the duration of the session — typically
10 hours by default.

---

## Getting access to resources

When the user wants to access a specific resource, they present their TGT
to the TGS in a request called the TGS-REQ. The TGS checks the TGT and
if it is valid, returns a service ticket in the TGS-REP. This service
ticket is specifically for that one resource, not for general access.

The user then presents that service ticket directly to the resource they
want to access. The resource verifies the ticket and grants access. The
user never has to enter their password again — the tickets handle
everything.

---

## Full authentication flow

```
1. User enters password and it gets hashed

2. AS-REQ — client sends hash to the Authentication Service

3. AS-REP — AS returns TGT and session key if hash matches

4. TGS-REQ — client presents TGT to the Ticket Granting Service
             to request access to a specific resource

5. TGS-REP — TGS returns a service ticket for that specific resource

6. Client presents service ticket directly to the resource

7. Access granted
```

---

## Kerberoasting

Kerberoasting is an attack that targets service accounts in Kerberos.
It does not require a compromised admin account to start — it only needs
any valid low-privilege domain user account.

The attacker uses that low-privilege account to request a service ticket
for a service account that has a Service Principal Name (SPN) registered.
The TGS returns that service ticket encrypted with the service account's
password hash. The attacker takes that encrypted ticket offline and uses
brute force or dictionary attacks to crack the hash and recover the
plaintext password.

The reason this is dangerous is that service accounts often have high
privileges and weak passwords that never expire — making them easier to
crack and more valuable once compromised.

**Defensive controls:**

- Use strong, long, randomly generated passwords on service accounts

- Use Group Managed Service Accounts (gMSA) which rotate passwords automatically

- Monitor for unusual TGS requests — a user requesting service tickets for services they never use is a detectable signal

- Apply least privilege to service accounts so even if cracked the blast radius is limited

---

## Pass the Ticket

Pass the Ticket is an attack where an attacker steals a Kerberos ticket
from a compromised machine and uses it to authenticate as the victim user
on other systems — without ever needing the user's password.

After a user authenticates, their TGT is stored in memory on their
machine. An attacker who gains access to that machine can use a tool like
Mimikatz to extract the ticket from memory and save it as a file. They
then load that ticket into their own session on a different machine. The
KDC treats them as the legitimate user because the ticket is valid — it
does not verify which machine the ticket is being presented from.

The attacker can access any resource the victim had access to for as long
as the ticket is valid, which is up to 10 hours by default.

This attack directly exploits two IAM weaknesses. First, over-privileged
accounts — a stolen Domain Admin ticket gives the attacker Domain Admin
access everywhere. Second, standing access — permanent elevated privileges
travel with the stolen ticket.

**Defensive controls:**

- Privileged Access Workstations — admin tickets should only ever be issued on hardened, isolated machines so they cannot be stolen from general-purpose workstations

- Tiered administration — separating admin accounts by tier means a compromised lower-tier account cannot be used to obtain higher-tier tickets

- Short ticket lifetimes — reducing the default 10 hour TGT lifetime limits how long a stolen ticket remains usable

- Credential Guard — a Windows security feature that uses virtualization to protect credentials and tickets in memory, making them significantly harder to extract

- Monitor for anomalous ticket usage — a ticket being used from a new IP address or unusual location is a detectable signal

---

## Questions to revisit

- [ ] What is a Service Principal Name (SPN) and how does it relate to Kerberoasting?

- [ ] How does Credential Guard technically prevent ticket extraction from memory?

- [ ] What is a Golden Ticket attack and how does it differ from Pass the Ticket?
