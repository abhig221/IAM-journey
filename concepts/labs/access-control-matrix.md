# Access Control Matrix: Umbrella Corporation

---

## Overview

This is an access control matrix for Umbrella Corporation, a fictional
medical, pharmaceutical, and consumer products conglomerate. It defines
what each of five user roles can do with five sensitive company resources.
I designed it around least privilege, separation of duties, and
just-in-time (JIT) access, so that every role has what it needs to do its
job and nothing more.

---

## Access levels

I used four access levels:

- **No Access** — the role cannot see or interact with the resource

- **Read Only** — the role can view the resource but not change it

- **Read/Write** — the role can view and modify the contents of the resource

- **Full Control** — the role can read, write, and manage the resource, including its permissions

**JIT** means just-in-time access. It is temporary, tied to a specific
need, approved, logged, and revoked once the task is finished. Anything
not marked JIT is standing access, meaning the role always has it.

Execute is not included because none of these resources are programs or
scripts. They are data repositories and systems, so there is nothing to run.

---

## The matrix

| User | Research Data Repository | Employee Records System | Compliance & Audit Logs | Customer Database | IT Infrastructure Console |
|---|---|---|---|---|---|
| Research Scientist | Full Control | No Access | Read Only | No Access | No Access |
| HR Manager | No Access | Full Control | JIT Read/Write | No Access | No Access |
| IT Administrator | No Access (JIT exception) | No Access (JIT exception) | No Access (JIT exception) | No Access (JIT exception) | Full Control |
| Compliance Officer | JIT Read Only | JIT Read Only | Full Control | JIT Read Only | JIT Read Only |
| Customer Support Rep | No Access | No Access | No Access | Read/Write | No Access |

---

## Rationale

### Research Scientist

The Research Scientist receives Full Control on the Research Data
Repository because they constantly read, write, and modify that data as
their core job function. They receive Read Only access to Compliance &
Audit Logs because they need to reference what regulations and standards
apply to their research, but they have no role in creating or modifying
compliance records themselves — that responsibility belongs to the
Compliance Officer. They have no access to Employee Records, the Customer
Database, or the IT Infrastructure Console since none of these fall within
the scope of their work.

### HR Manager

The HR Manager receives Full Control of the Employee Records System
because their job consists of hiring employees, training them, and
determining their pay and compensation. To do that job fully, they need to
be able to create, modify, and manage those records. They receive JIT
Read/Write access to Compliance & Audit Logs because there will be times
when they investigate a specific employee and need to document the
findings alongside the Compliance Officer. This need is occasional and
tied to individual cases, so it is granted just-in-time and expires when
the case is documented, rather than as standing access. The Compliance
Officer remains the owner of the logs, which keeps the separation of
duties intact. They have no access to the other resources because they
fall outside HR's work.

### IT Administrator

The IT Administrator receives Full Control of the IT Infrastructure
Console because most of their time is spent configuring, maintaining,
monitoring, and fixing the systems that the rest of the company depends
on. They have no standing access to the Research Data Repository,
Employee Records System, Compliance & Audit Logs, or Customer Database.
They manage the infrastructure that stores, transports, and gives other
groups access to data, but they do not need the sensitive data itself to
do that, so giving it to them would violate least privilege. The exception
is when data becomes corrupted and a department needs IT's help to restore
or recover it. In that case the IT Administrator is granted temporary JIT
access to that specific resource, approved by the department that owns the
data and logged. Once the recovery is finished, the access is revoked.

### Compliance Officer

The Compliance Officer receives Full Control of Compliance & Audit Logs
because creating, maintaining, and protecting those records is their core
responsibility. For every other resource, they receive JIT Read Only
access. An audit is something that happens at specific times, so they only
need to see the Research Data Repository, Employee Records System,
Customer Database, and IT Infrastructure Console while an audit or
investigation is in progress. Their access is read only on purpose. An
auditor who can change the systems they are auditing could alter the
evidence, which would make the audit meaningless. This is separation of
duties: the people who do the work and the person who checks the work must
not be able to change each other's records. When the audit is complete,
the access expires.

### Customer Support Rep

The Customer Support Rep receives Read/Write access to the Customer
Database because helping customers all day means constantly looking up
accounts and updating them, such as changing contact information,
logging issues, and recording resolutions. This is standing access rather
than JIT because the need is continuous. Making them request access every
time a customer calls would slow down their work and add no real security
benefit. They do not get Full Control because they should not be able to
manage the database itself or change its permissions. They have no access
to the other four resources, which have nothing to do with supporting
customers.

---

## Scenarios

**Corrupted research files.** Research data becomes corrupted and the
Research Scientist cannot recover it. The IT Administrator requests JIT
access to the Research Data Repository, the data owner approves it, IT
restores the files, and the access is revoked. The request and everything
done during it is logged.

**Employee investigation.** The HR Manager investigates a specific
employee and needs to document the findings. They request JIT Read/Write
access to Compliance & Audit Logs for that case. Once it is documented,
the access expires and the Compliance Officer remains the owner of the
record.

**Quarterly audit.** The Compliance Officer begins an audit and receives
JIT Read Only access to the resources being reviewed. They can examine
everything but cannot change anything they are auditing. When the audit
ends, the access is removed.

---

## Why four levels instead of Windows permissions

Windows NTFS uses six basic permissions: Full Control, Modify, Read &
Execute, List Folder Contents, Read, and Write. That level of detail
makes sense when you are configuring a file server, but it is too granular
for describing business roles. I simplified it to four levels that map
onto it:

- No Access = no permissions granted

- Read Only = Read

- Read/Write = Read and Write (similar to Modify)

- Full Control = Full Control, including changing permissions and ownership

Read & Execute and List Folder Contents were left out because they apply
to programs and folder browsing, not to the kind of data resources in this
matrix.

---

## How this applies in a real organization

A matrix like this is a design document. In practice it would be
implemented in a few ways:

- **RBAC** — each row becomes a role (Research Scientist, HR Manager, etc.) and users are assigned to the role instead of being given permissions one by one

- **JIT access** — handled by a privileged access management (PAM) tool or an approval workflow, where a request is approved, the access is time-limited, and the session is logged

- **ABAC** — conditions can be added on top of roles, such as requiring an approved ticket, a specific time window, or a managed device before JIT access is granted

- **Access reviews** — the matrix is the baseline that managers and compliance teams compare actual permissions against during periodic access certifications, to catch over-provisioned accounts and permissions that were never removed

---

## Questions to revisit

- [ ] How would this matrix change if the Research Scientist role were split into junior and senior roles?

- [ ] What tool would actually enforce the JIT approvals and automatic expiration?

- [ ] How often should access to each resource be reviewed?
