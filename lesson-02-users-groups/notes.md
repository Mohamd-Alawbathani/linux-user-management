# Lesson 2 — Users, Groups, Privilege Escalation, and Troubleshooting

This lesson focuses on Linux user administration, group management, privilege escalation, and troubleshooting common account-related problems.

The lesson is divided into four main topics:

- **2A — Manage User Accounts**
- **2B — Manage Group Accounts**
- **2C — Configure Privilege Escalation**
- **2D — Troubleshoot User and Group Issues**

The goal is to understand how Linux identifies users, organizes permissions through groups, delegates administrative privileges, and provides tools for diagnosing account and login problems.

---

# Topic 2A — Manage User Accounts

Topic 2A focuses on creating, modifying, deleting, and configuring Linux user accounts.

Linux stores important user information in several configuration files.

---

## User Account Files

### `/etc/passwd`

The `/etc/passwd` file stores basic information about user accounts.

A typical entry looks like:

```text
username:x:1001:1001:Full Name:/home/username:/bin/bash
```

The fields represent:

1. Username
2. Password placeholder
3. UID
4. GID
5. Comment or full name
6. Home directory
7. Login shell

Example:

```text
student:x:1000:1000:Student:/home/student:/bin/bash
```

The `x` indicates that protected password information is stored in `/etc/shadow`.

---

### `/etc/shadow`

The `/etc/shadow` file stores protected password information.

It also contains password aging information such as:

- Last password change
- Minimum password age
- Maximum password age
- Password expiration warning period
- Account expiration information

Because this file contains sensitive information, normal users cannot read it.

---

## Creating Users

The `useradd` command creates a new user account.

Example:

```bash
sudo useradd student1
```

To create a home directory:

```bash
sudo useradd -m student1
```

Common options include:

```text
-c    Set a comment or full name
-e    Set an account expiration date
-m    Create a home directory
-s    Set the login shell
-u    Specify a UID
```

Example:

```bash
sudo useradd -m -c "Student One" -s /bin/bash student1
```

---

## The `adduser` Command

Some Linux distributions provide the `adduser` command.

Example:

```bash
sudo adduser student1
```

Unlike `useradd`, `adduser` is usually interactive.

It may automatically:

- Create the user
- Create the user's primary group
- Create the home directory
- Copy files from `/etc/skel`
- Ask for a password
- Ask for user information

---

## Setting and Changing Passwords

The `passwd` command is used to set or change passwords.

To change the current user's password:

```bash
passwd
```

To change another user's password:

```bash
sudo passwd student1
```

Useful options include:

```text
-l    Lock the password
-u    Unlock the password
-e    Expire the password immediately
```

Example:

```bash
sudo passwd -l student1
```

---

## Modifying Users

The `usermod` command modifies an existing user.

Change the comment:

```bash
sudo usermod -c "Student One" student1
```

Change the login shell:

```bash
sudo usermod -s /bin/bash student1
```

Set an account expiration date:

```bash
sudo usermod -e 2026-12-31 student1
```

Change the username:

```bash
sudo usermod -l newname oldname
```

---

## Password Aging with `chage`

The `chage` command manages password aging and account expiration settings.

Display current settings:

```bash
sudo chage --list student1
```

Set the minimum number of days between password changes:

```bash
sudo chage -m 1 student1
```

Set the maximum password age:

```bash
sudo chage -M 90 student1
```

Set the warning period:

```bash
sudo chage -W 7 student1
```

Set the account expiration date:

```bash
sudo chage -E 2026-12-31 student1
```

---

## Deleting Users

To delete a user account:

```bash
sudo userdel student1
```

To delete the user and the user's home directory:

```bash
sudo userdel -r student1
```

The `-r` option should be used carefully because user data is removed.

---

## Topic 2A Summary

Topic 2A introduced the core Linux user management tools:

```bash
useradd
adduser
usermod
userdel
passwd
chage
```

Important files include:

```text
/etc/passwd
/etc/shadow
/etc/login.defs
/etc/skel
```

The main goal is to understand how Linux creates, identifies, configures, and secures user accounts.

---

# Topic 2B — Manage Group Accounts

Topic 2B focuses on Linux groups.

Groups allow administrators to assign permissions to multiple users efficiently.

Instead of configuring permissions for every individual user, users can be placed into groups and permissions can be assigned to the group.

---

## The `/etc/group` File

Linux group information is stored in:

```text
/etc/group
```

A typical entry looks like:

```text
groupname:x:1010:user1,user2
```

The fields represent:

1. Group name
2. Password placeholder
3. GID
4. Group members

Example:

```text
developers:x:1010:ali,mohammed
```

---

## UID and GID

Linux identifies users and groups by numbers.

```text
UID = User ID
GID = Group ID
```

The username and group name are human-readable labels, while Linux internally relies heavily on UID and GID values.

---

## Creating Groups

The `groupadd` command creates a new group.

Example:

```bash
sudo groupadd developers
```

---

## Modifying Groups

The `groupmod` command modifies an existing group.

To rename a group:

```bash
sudo groupmod -n engineering developers
```

This changes the group name from `developers` to `engineering`.

---

## Deleting Groups

The `groupdel` command removes an existing group.

Example:

```bash
sudo groupdel engineering
```

A group cannot normally be removed while it is the primary group of an existing user.

---

## Adding Users to Groups

Users can belong to multiple groups.

To add a user to an additional group:

```bash
sudo usermod -aG developers ali
```

The options are important:

```text
-a    Append the user to the group
-G    Specify supplementary groups
```

The `-a` option is extremely important.

Without `-a`, using `-G` may replace the user's existing supplementary group memberships.

Correct:

```bash
sudo usermod -aG developers ali
```

Potentially dangerous:

```bash
sudo usermod -G developers ali
```

---

## Checking Group Membership

To display the groups a user belongs to:

```bash
groups ali
```

Another useful command is:

```bash
id ali
```

The `id` command can display:

- UID
- Primary GID
- Supplementary groups

---

## Primary and Supplementary Groups

A user has one primary group and may have several supplementary groups.

The primary group is commonly used when creating files.

Supplementary groups provide additional access permissions.

---

## Topic 2B Summary

The main group management commands are:

```bash
groupadd
groupmod
groupdel
usermod -aG
groups
id
```

Important concepts include:

```text
GID
Primary Group
Supplementary Groups
/etc/group
```

The main purpose of groups is to make permission management more efficient.

---

# Topic 2C — Configure Privilege Escalation

Topic 2C focuses on elevated administrative privileges.

Linux users should normally operate with standard privileges and only elevate their permissions when required.

This follows the security principle known as:

```text
Principle of Least Privilege
```

A user should receive only the permissions required to complete a task.

---

## The Root User

The `root` account is the most powerful account on a Linux system.

Root can:

- Modify system files
- Manage users and groups
- Install software
- Change permissions
- Stop services
- Shut down the system

Because root has unrestricted privileges, using the root account for normal daily work increases risk.

---

## Using `su`

The `su` command switches to another user.

Example:

```bash
su root
```

This changes the user identity but may preserve parts of the original environment.

Using:

```bash
su - root
```

starts a full login shell for root and loads root's environment.

Without specifying a username:

```bash
su -
```

Linux normally assumes root.

---

## Using `sudo`

The `sudo` command allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo useradd student1
```

Instead of switching permanently to root, only the required command receives elevated privileges.

This is safer and provides better administrative control.

---

## Checking Sudo Permissions

To see which commands the current user can execute with sudo:

```bash
sudo -l
```

This displays the sudo privileges assigned to the user.

---

## Repeating the Previous Command with Sudo

Bash can repeat the previous command using:

```bash
!!
```

Example:

```bash
sudo !!
```

This repeats the previous command with `sudo`.

---

## The `/etc/sudoers` File

Sudo permissions are configured in:

```text
/etc/sudoers
```

A rule may look like:

```text
mohammed ALL=(ALL:ALL) ALL
```

This means that the user `mohammed` can execute all commands through sudo.

---

## Using `visudo`

The recommended way to edit the sudoers configuration is:

```bash
sudo visudo
```

`visudo` checks the syntax before saving changes.

This reduces the risk of breaking sudo access.

The sudoers file should not normally be edited directly with a regular editor.

---

## `NOPASSWD`

A sudo rule may contain:

```text
NOPASSWD:
```

This allows an authorized command to be executed without prompting for the user's password.

Example:

```text
mohammed ALL=(ALL) NOPASSWD: /usr/sbin/shutdown
```

This does not remove the user's password.

It only removes the password prompt for the specified sudo command.

---

## Administrative Groups

Some distributions use an administrative group such as:

```text
sudo
```

Ubuntu commonly uses the `sudo` group.

Other distributions may use:

```text
wheel
```

Users in these groups may be allowed to perform administrative tasks depending on the sudoers configuration.

---

## `sudoedit`

The `sudoedit` command safely edits protected files.

Example:

```bash
sudoedit /etc/hosts
```

This is often preferable to running the text editor itself as root.

---

## PolicyKit and `pkexec`

Linux also provides PolicyKit, commonly known as:

```text
polkit
```

Polkit provides fine-grained authorization for specific administrative actions.

A related command is:

```bash
pkexec
```

It allows an authorized user to execute a program with elevated privileges according to polkit rules.

Other polkit tools include:

```text
pkaction
pkcheck
pkttyagent
```

---

## Authentication vs Authorization

These are two important security concepts.

```text
Authentication
```

answers:

> Who are you?

```text
Authorization
```

answers:

> What are you allowed to do?

A user may successfully authenticate but still lack authorization to perform an administrative action.

---

## Topic 2C Summary

The main privilege escalation tools include:

```bash
su
su -
sudo
sudo -l
visudo
sudoedit
pkexec
```

Important concepts include:

```text
root
Least Privilege
/etc/sudoers
sudo group
wheel group
polkit
Authentication
Authorization
```

The main goal is to provide administrative access without giving users unnecessary unrestricted root privileges.

---

# Topic 2D — Troubleshoot User and Group Issues

Topic 2D focuses on diagnosing problems involving users, groups, processes, passwords, and login activity.

Troubleshooting should be performed logically.

The administrator should identify the cause of a problem before attempting to fix it.

---

## Troubleshooting Account Creation Problems

If a user or group cannot be created, verify:

- The administrator has sufficient privileges
- The username or group name is not already in use
- The UID is not already in use
- The GID is not already in use

Useful files include:

```text
/etc/passwd
/etc/group
```

Example:

```bash
grep '^username:' /etc/passwd
```

---

## Checking User Processes

A user may have active processes that interfere with account management.

To display processes owned by a user:

```bash
ps -u username
```

If necessary, the processes can be terminated using:

```bash
sudo killall -u username
```

This command should be used carefully because it terminates processes belonging to that user.

---

## Troubleshooting Login Problems

If a user cannot log in, investigate the problem step by step.

Check whether the account exists:

```bash
grep '^username:' /etc/passwd
```

Check the account status:

```bash
sudo passwd -S username
```

Reset the password if required:

```bash
sudo passwd username
```

Check password aging and expiration:

```bash
sudo chage --list username
```

Possible login problems include:

- User account does not exist
- Incorrect password
- Password is expired
- Account is locked
- Account is expired
- Incorrect login shell
- Account configuration problem

---

## The Login Process

A simplified Linux login process includes:

1. Linux boots and loads the kernel.
2. The user enters a username and password.
3. Linux validates the account and authentication information.
4. Password expiration and account restrictions are checked.
5. The user's environment and profile are loaded.
6. The user receives an authenticated session.

Understanding this process helps administrators identify where a login failure occurs.

---

## `lastlog2`

On newer systems, `lastlog2` can display the latest login information for users.

Example:

```bash
lastlog2
```

This can help identify unused accounts or determine when users last logged in.

---

## `last`

The `last` command displays historical login and logout information.

Example:

```bash
last
```

It may display:

- Login sessions
- Logout times
- Session duration
- Reboots
- System boot events

---

## `who`

The `who` command displays users who currently have login sessions.

Example:

```bash
who
```

---

## `w`

The `w` command displays active users with additional session information.

Example:

```bash
w
```

It may show:

- Username
- Terminal
- Login time
- Idle time
- CPU usage
- Current command

---

## Login Command Comparison

```text
lastlog2
```

Shows the latest login information for users.

```text
last
```

Shows historical login and logout sessions.

```text
who
```

Shows users with current login sessions.

```text
w
```

Shows current users with additional session and activity information.

---

## Topic 2D Summary

Useful troubleshooting commands include:

```bash
grep
id
groups
ps
killall
passwd
chage
lastlog2
last
who
w
```

A good troubleshooting process is:

```text
Identify the problem
        ↓
Gather information
        ↓
Find the cause
        ↓
Apply the correct solution
        ↓
Verify the result
```

The most important principle is to avoid making random changes before identifying the actual cause of the problem.

---

# Lesson 2 Command Summary

## User Management

```bash
sudo useradd -m username
sudo adduser username
sudo usermod -c "Full Name" username
sudo usermod -s /bin/bash username
sudo passwd username
sudo chage --list username
sudo userdel username
sudo userdel -r username
```

## Group Management

```bash
sudo groupadd groupname
sudo groupmod -n newname oldname
sudo groupdel groupname
sudo usermod -aG groupname username
groups username
id username
```

## Privilege Escalation

```bash
su root
su - root
sudo command
sudo -l
sudo !!
sudo visudo
sudoedit /path/to/file
pkexec command
```

## Troubleshooting

```bash
grep '^username:' /etc/passwd
grep '^groupname:' /etc/group
ps -u username
sudo killall -u username
sudo passwd -S username
sudo chage --list username
lastlog2
last
who
w
```

---

# What I Learned

By completing Lesson 2, I learned:

- How Linux stores and identifies user accounts.
- How to create, modify, and delete users.
- How to configure passwords and account expiration.
- How Linux groups simplify permission management.
- How to create, modify, and delete groups.
- How to safely add users to supplementary groups.
- The difference between UID and GID.
- The difference between primary and supplementary groups.
- How root privileges work.
- How to use `su` and `sudo`.
- Why the principle of least privilege is important.
- How to safely modify sudo permissions using `visudo`.
- How to edit protected files using `sudoedit`.
- How PolicyKit provides fine-grained administrative authorization.
- The difference between authentication and authorization.
- How to troubleshoot user and group management problems.
- How to inspect and terminate user processes.
- How to troubleshoot login failures.
- How to investigate previous and current login activity.
- How to use Linux administrative commands in a structured troubleshooting process.

---

# Conclusion

Lesson 2 provided a complete introduction to Linux user and group administration.

I practiced the full account management lifecycle, including creating users, configuring passwords, managing groups, assigning administrative privileges, and troubleshooting common account and login problems.

The most important lesson was that Linux administration is not only about knowing commands. It is also about understanding how users, groups, permissions, authentication, and system configuration work together.

A Linux administrator should apply the principle of least privilege, verify changes after making them, and troubleshoot problems logically before applying a solution.
