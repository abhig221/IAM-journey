# Windows Fundamentals — TryHackMe (Part 1 and Part 2)

---

## Part 1

### Windows versions

The room starts by covering the different Windows versions, their
shortcomings, and how long each was supported. The main takeaway is the
difference between Windows 11 Home and Pro — Pro offers BitLocker, which
locks down the computer and its data in the event it gets stolen.

---

### The Windows GUI

The next section covers the components of the Windows GUI and what each
is used for — the Desktop, Start menu, taskbar, task view, search bar
(also known as Cortana), toolbars, and notification area.

---

### The file system — NTFS

Most Windows devices use NTFS, or New Technology File System. Its main
feature is that files and folders can be repaired or recovered using a
stored log file. This is possible because of the journaling file system,
which logs changes before they are written to the main drive — keeping
data safe in the event of a power outage or other disturbance during a
write.

NTFS also allows permissions to be set on specific files and folders.
The main permissions Windows offers are:

- Read

- Write

- Read and execute

- List folder contents

- Modify

- Full control

Each of these permissions is technically an Access Control Entry (ACE)
inside that file or folder's DACL — the same DACL concept covered in
`windows-acls.md`. You can view a file's permissions and see the groups
associated with each one directly through the Security tab.

---

### User accounts and UAC

Windows offers two main account types — Admin and Standard User. Admin
accounts can make changes to everything, while Standard User accounts
can only make changes to folders and files they have been explicitly
given access to.

You can manage accounts through the Start menu by searching "otheruser,"
or with the shortcut `lusrmgr.msc` through Run.

This section also covers UAC, or User Account Control. UAC adds an extra
confirmation step any time an action requires administrator-level
privileges — even for an account that is already an admin. This connects
directly to the least privilege principle covered in
`concepts/least-privilege.md` — UAC exists so that admin accounts do not
operate with full privileges by default at all times. It is the same
just-in-time mindset behind tools like PIM: elevated access is only
active for the moment it is actually needed, not standing at all times.

---

## Part 2

### System Configuration (MSConfig)

Part 2 builds on the file system and permissions knowledge from Part 1.
It starts with System Configuration, accessed through the shortcut
`msconfig`, used for advanced troubleshooting. Local administrator
rights are required to use it.

**General tab** — controls which devices and services start up at the
same time as the system. Options are Normal, Diagnostic, and Selective
startup.

**Boot tab** — allows the admin to boot the computer in different modes
to help pinpoint specific issues.

**Services tab** — lists every service on the system, who provides it,
and whether it is running, stopped, or disabled (including the date it
was disabled if applicable).

**Startup tab** — leads directly to Task Manager, since that is where
startup applications are actually managed on modern Windows. On the
TryHackMe lab machine specifically, this tab does not show the usual
startup items because the lab runs on Windows Server rather than a
standard Windows 10 or 11 install. To see what actually starts up on a
Windows Server system, use Run and type `shell:startup`.

**Tools tab** — provides a list of additional tools available for
configuring the operating system.

---

### Advanced system settings

This section also covers Advanced System Settings, which can be used to
configure the page file — useful in the event the computer runs low on
RAM. Advanced System Settings also lets you view and change the write
debug information, which controls what happens in the event of a system
failure or crash.

---

## Questions to revisit

- [ ] How does UAC's prompt behavior differ between a Standard User and
      an Admin account triggering the same elevated action?

- [ ] What specific information gets written to the debug log during a
      system crash and where is it stored?

- [ ] How does the page file interact with RAM under memory pressure?
