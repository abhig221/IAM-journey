# Linux File Permissions

## Overview
Linux controls access to files and directories through a permission
system that applies to three groups — the owner, the group, and others.
Understanding how to read and modify these permissions is fundamental
to implementing least privilege at the operating system level.

---

## Commands

**chmod** lets you change the permissions of a file or directory for
the owner, group, and others.

**chown** lets you change both the owner and the group of a file or
directory.

**chgrp** lets you change just the group that has access to a file
without changing the owner.

---

## Permission notations

**Octal notation** is a way to set permissions using numbers where
r=4, w=2, and x=1. You add the values together for each permission
set. So rwx=7, rw-=6, r-x=5, r--=4. A permission of 754 means the
owner has rwx, the group has r-x, and others have r-- only.

Example: `chmod 754 file.txt`

**Symbolic notation** is another way to change permissions but instead
of setting all permissions at once it lets you modify specific ones
individually using letters and operators.

- `u+x` adds execute permission for the owner
- `g-w` removes write permission from the group
- `o=r` sets others to read only

Example: `chmod u+x file.txt`

The key difference is octal replaces all permissions in one go while
symbolic modifies specific ones without touching the rest.

---

## Special permissions

**setuid** runs a file with the permissions of the file owner instead
of the user running it. Used when a file needs elevated permissions
to execute properly. A common example is the passwd command — regular
users run it but it needs root access to modify system files.

**setgid** applies group permissions to a file or directory. When set
on a directory, new files created inside it automatically inherit that
directory's group instead of the creator's primary group.

**Sticky bit** lets users write and modify files in a shared directory
but only the owner of a file can delete it. The `/tmp` directory uses
sticky bit so users can create files there but cannot delete each
other's files.

---

## umask

umask sets the default permission mask applied to every new file or
directory you create. It subtracts from the maximum permissions
automatically so you don't have to manually chmod every new file.
The standard umask of 022 means new files get 644 (rw-r--r--) and
new directories get 755 (rwxr-xr-x) by default.

Run `umask` in the terminal to see your current value.

---

## Least privilege at the OS level

Linux permissions are one of the primary ways least privilege is
enforced at the operating system level. A file that only needs to be
read should not be writable. A script that only needs to run for one
user should not be executable by everyone. setuid should be used as
sparingly as possible because files that run with elevated permissions
are a common privilege escalation target.

---

## Lab notes
_(paste your terminal outputs and observations here)_

---

## Questions to revisit
- [ ] How does setuid create a privilege escalation risk and what
      controls limit it?
- [ ] What does find / -perm -4000 do and why is it useful for
      security auditing?
- [ ] How does umask interact with chmod when both are applied?
