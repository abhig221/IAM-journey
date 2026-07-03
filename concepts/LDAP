# LDAP (Lightweight Directory Access Protocol)

---

## Overview

LDAP is a protocol used to query and manage directory information —
things like users, groups, and computers stored in a directory service
like Active Directory. Its primary purpose is looking up and modifying
directory objects, not authentication itself. Authentication can happen
through LDAP but that is a secondary use, not what it was designed for.

---

## How it is structured

LDAP is set up in a tree-like hierarchy. The theoretical model it was
based on starts with a root directory and goes down through countries,
organizations, organization sites, and individuals. In practice,
especially in Active Directory, the hierarchy looks like this:

- Domain components at the top (the domain itself)

- Organizational units below that (departments, locations, teams)

- Objects at the bottom (users, groups, computers)

---

## How LDAP stores data

LDAP works based on entries. Every object in the directory is an entry
and each entry has three main components:

- A Distinguished Name (DN) — identifies exactly where the entry sits in the hierarchy

- A collection of attributes — the actual data about the object, like a user's email or job title

- A collection of object classes — defines what type of object this is and what attributes it can have

---

## DN structure

The DN is the full address of an entry in the directory, combining all
components from most specific to least specific. It is made up of the
following parts:

**DC — Domain Component** represents the domain itself broken into
parts. So company.com becomes DC=company,DC=com — each part of the
domain name separated by dots becomes its own DC component.

**OU — Organizational Unit** works like a folder and is used to
separate and organize objects. A company might use OU=IT and OU=HR
to separate departments.

**CN — Common Name** represents the actual object — a user, group, or
computer. A user named John Doe would be CN=John Doe.

**DN** is the full combination of all of the above, written from most
specific to least specific. So John Doe in the IT department at
company.com would look like this:

CN=John Doe,OU=IT,DC=company,DC=com

Read it right to left to understand where something lives — company.com
domain, inside the IT organizational unit, the object named John Doe.

---

## Example LDAP tree

```
DC=company, DC=com
│
├── OU=IT
│   ├── CN=John Doe
│   └── CN=Jane Smith
│
├── OU=HR
│   └── CN=Bob Jones
│
└── OU=Servers
    └── CN=DC01
```

---

## Why LDAP matters for IAM

Active Directory uses LDAP as the protocol for querying directory
information. When an application needs to look up whether a user exists,
what groups they belong to, or what their attributes are, it sends an
LDAP query to the domain controller. Understanding LDAP structure is
essential for IAM because every provisioning system, every access review
tool, and every identity platform that integrates with AD speaks LDAP.

---

## Questions to revisit

- [ ] What does an actual LDAP search filter look like and how do you read one?

- [ ] How does LDAP differ from LDAPS and why does it matter for security?

- [ ] How does AD use LDAP differently from how a standalone LDAP server works?
