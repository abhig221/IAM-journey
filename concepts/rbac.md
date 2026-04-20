# RBAC (Role-Based Access Control)

## Overview
RBAC is a system that helps manage who or what has access to certain 
resources and the level of access they have.

## Security principal
A security principal is an object that represents a user, group, 
service principal, or managed identity.

## Role
A role is assigned to a security principal and allows a certain level 
of access to everyone assigned that role. The level of access is based 
on the permissions defined within that role.

## Role assignments
Role assignment is the process of assigning a role to a user, group, 
service principal, or managed identity based on scope. Every role 
assignment includes three components: the security principal (who), 
the role definition (what permissions), and the scope (where those 
permissions apply).

## Role hierarchies
Role hierarchies define how roles and their permissions flow through 
groups and users based on their relationships. They follow a descending 
parent-child structure — a role lower in the hierarchy inherits the 
permissions of its parent. However, permissions can be explicitly denied 
at lower levels, overriding and removing what was inherited.

## Role explosion
Role explosion occurs when an organization creates roles that are too 
specific, resulting in too many highly granular roles that become 
difficult to manage and assign. This is RBAC's core limitation and the 
primary reason ABAC exists.

## Diagram

```
USERS                    ROLES                    RESOURCES
─────                    ─────                    ─────────

Alice ──────────────► HR Manager ──────────────► Employee Records
Bob ─────────────────►           ──────────────► Payroll System

Carol ───────────────► Developer ──────────────► Code Repository
Dave ────────────────►           ──────────────► Dev Database

Eve ─────────────────► Read Only ──────────────► Reports Dashboard
```

## Role hierarchy

```
IT Admin (parent)
│  └─ permissions: full system access
│
├──► Network Engineer (child)
│    └─ inherits: full system access
│    └─ adds: network config permissions
│
└──► Help Desk (child)
     └─ inherits: full system access
     └─ DENY: cannot modify user accounts
```
