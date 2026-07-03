# DNS for IAM

---

## Overview

DNS, or Domain Name System, is essentially the phone book of a network.
It translates human-readable domain names into IP addresses that machines
can actually use to communicate. Without DNS, every device would need to
know the exact IP address of every other device it wants to talk to.

---

## Record types

**A records** direct a domain name to an IPv4 address. When your computer
looks up a domain controller by name, it gets back an IP address via an
A record.

**PTR records** are the opposite of A records — they direct an IP address
back to a domain name. They matter for IAM because authentication logs
record IP addresses, and PTR records let administrators reverse-lookup
those addresses to identify which machine generated a security event.

**SRV records** point to a specific server and service using a port number.
They are used when a service on a server needs to be discoverable by
clients. Active Directory uses SRV records to advertise where Kerberos
and LDAP services are running so that clients can find them automatically.

---

## DNS zones

DNS zones are used to delegate administrative control over a portion of
the DNS namespace. A zone is a boundary of authority — so company.com
might be one zone managed by the IT team, and sales.company.com might
be a separate zone delegated to a different administrator. This makes
large DNS environments easier to manage by dividing responsibility.

---

## Why DNS matters for IAM

DNS is the foundation that Kerberos and Active Directory sit on. When a
user tries to log in, their computer queries DNS to find the domain
controller. When Kerberos needs to issue a ticket, the client queries DNS
to find the Key Distribution Center. Both of these lookups depend on SRV
records being present and correct.

If DNS is broken or misconfigured — SRV records missing, A records stale,
or records not updated after a server IP change — Kerberos cannot find
the KDC and authentication fails entirely. Users cannot log in, Group
Policy cannot apply, and the entire authentication chain collapses at
the first step.

This is why in any AD troubleshooting scenario, DNS is always the first
thing you check.

---

## MXToolbox exercise

I tried looking up _kerberos._tcp.dc._msdcs.contoso.com on MXToolbox
and got DNS Record not found. This is because contoso.com is a
placeholder domain Microsoft uses in documentation — it does not exist
publicly. When I tried looking up real Microsoft domains, the SRV records
still did not resolve publicly.

This is actually correct security practice. Internal AD SRV records are
intentionally private and not publicly resolvable — exposing them would
advertise internal infrastructure to anyone scanning the internet. The
fact that these records do not show up publicly is by design, not a
misconfiguration.

---

## Questions to revisit

- [ ] What exactly happens step by step when a Kerberos SRV record lookup fails?

- [ ] How does AD automatically register and update SRV records in DNS when a domain controller comes online?
