# ABAC (Attribute-Based Access Control)

## Overview
Attribute-based access is a way of securely granting access of certain 
resources (objects) to certain users (subjects) based on what the object 
is, what the subjects are, and making sure the environment attributes 
match accordingly.

## Policy engines
Policy engines are the software that carry out the process of granting 
access based on attributes. The policy engine is essentially the PDP in 
action, and XACML is the language it reads to make decisions.

## When to use ABAC over RBAC
ABAC is used instead of RBAC when access needs to be granted on a more 
nuanced basis compared to just roles. It is a more specific way of 
granting access.

## XACML
XACML is the industry standard language for writing the rules that tell 
the system what to do with attributes.

### The four components
**PEP (Policy Enforcement Point)** — the gatekeeper of the resource the 
user is trying to access. Its only role is to grant or deny access based 
on what the PDP tells it.

**PDP (Policy Decision Point)** — checks the attributes of whoever is 
requesting access and grants or denies access based on those attributes.

**PIP (Policy Information Point)** — gives the necessary attribute 
information to the PDP so it can make its decision.

**PAP (Policy Administration Point)** — the admin interface where 
security teams define the access rules.

## RBAC vs ABAC comparison

| Factor | RBAC | ABAC |
|--------|------|------|
| Access based on | Role assigned to user | Attributes of user, resource, environment |
| Flexibility | Low — role defines everything | High — any attribute combination |
| Complexity | Simple to manage at small scale | More complex to design and maintain |
| Best for | Stable, well-defined job functions | Dynamic, context-sensitive access needs |
| Core limitation | Role explosion at scale | Policy complexity at scale |
| Example | All HR staff can access payroll | Doctors can access records only for their patients during business hours |

## REQUEST FLOW
────────────

User (subject)                          Resource (object)
─────────────                           ────────────────
name: Dr. Smith          ──────────►    type: patient record
department: cardiology   ACCESS REQ     sensitivity: high
role: doctor                            owner: cardiology dept
clearance: level 2
                              │
                              ▼
                    ┌─────────────────┐
                    │      PEP        │
                    │  (gatekeeper)   │
                    └────────┬────────┘
                             │ forward request
                             ▼
                    ┌─────────────────┐
                    │      PDP        │◄──── XACML Policy
                    │  (decision)     │      "doctors can access
                    └────────┬────────┘       cardiology records
                             │ needs attrs    during business hours"
                             ▼
                    ┌─────────────────┐
                    │      PIP        │
                    │  (fetch attrs)  │
                    └────────┬────────┘
                             │ returns:
                             │ • patient assigned to Dr. Smith ✓
                             │ • current time: 2pm weekday ✓
                             │ • dept match: cardiology ✓
                             ▼
                    ┌─────────────────┐
                    │      PDP        │
                    │  decision:      │
                    │  PERMIT         │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │      PEP        │
                    │  enforces:      │
                    │  ACCESS GRANTED │
                    └─────────────────┘
