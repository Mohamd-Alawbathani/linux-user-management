# Project 2D - Troubleshooting User and Group Issues

In this project, I practiced troubleshooting Linux user and group issues.

The goal of this project was to learn how to verify users and groups, inspect user processes, check account status, investigate login activity, and identify common user account problems.

---

## 1. Verifying Group Information

I verified that the `halk` group exists by checking the `/etc/group` file.

### Command

```bash
grep '^halk:' /etc/group
```

The `^halk:` pattern searches specifically for a group entry that begins with `halk:`.

The output confirmed that the group exists and displayed its GID.

### Screenshot

![Verifying Group Information](2D-group-members-verification.png)

---

## 2. Checking User Processes

I checked the processes running under the `halk` user account.

### Command

```bash
ps -u halk
```

This command displays processes owned by the specified user.

Checking active processes is useful when troubleshooting user account problems or before deleting a user.

### Screenshot

![Checking User Processes](2D-ps-user-processes.png)

---

## 3. Terminating User Processes

I practiced terminating processes owned by the `halk` user.

### Command

```bash
sudo killall -u halk
```

The `-u` option specifies the user whose processes should be terminated.

This can be useful when active processes prevent a user account from being modified or deleted.

### Screenshot

![Terminating User Processes](2D-killall-user-processes.png)

---

## 4. Verifying a User Account

I verified that the `halk` user account exists by checking the `/etc/passwd` file.

### Command

```bash
grep '^halk:' /etc/passwd
```

The output confirmed that the user exists.

Example output:

```text
halk:x:1009:1010::/home/halk:/bin/bash
```

This entry contains information such as:

- Username
- Password placeholder
- UID
- GID
- Home directory
- Login shell

### Screenshot

![Verifying a User Account](2D-passwd-user-verification.png)

---

## 5. Checking User Account Status

I checked the password and account status of the `halk` user.

### Command

```bash
sudo passwd -S halk
```

The `-S` option displays password status information for the account.

This can help determine whether the user's password is configured or locked.

### Screenshot

![Checking User Account Status](2D-passwd-account-status.png)

---

## 6. Checking the Last Login

I used `lastlog2` to check the latest login information recorded for users on the system.

### Command

```bash
lastlog2
```

The output can display information such as:

- Username
- Terminal
- Remote host
- Latest login date
- Latest login time

This is useful for identifying when a user last logged into the system.

### Screenshot

![Checking the Last Login](2D-lastlog-last-login.png)

---

## 7. Reviewing Login History

I used the `last` command to review previous login and logout activity.

### Command

```bash
last
```

The `last` command can display:

- User login sessions
- Logout times
- Session duration
- System boots
- System reboots

This information is useful when investigating previous user activity.

### Screenshot

![Reviewing Login History](2D-last-login-history.png)

---

## 8. Checking Currently Active Users

I used the `w` command to view users who currently have active sessions on the system.

### Command

```bash
w
```

The output can include:

- Username
- Terminal
- Login time
- Idle time
- CPU usage
- Current process or command

This is useful for determining who is currently logged in and whether their session is active or idle.

### Screenshot

![Checking Currently Active Users](2D-w-active-users.png)

---

## Commands Practiced

```bash
grep '^halk:' /etc/group

ps -u halk

sudo killall -u halk

grep '^halk:' /etc/passwd

sudo passwd -S halk

lastlog2

last

w
```

---

## Important Files

### `/etc/passwd`

Stores basic Linux user account information.

```text
/etc/passwd
```

---

### `/etc/group`

Stores Linux group information.

```text
/etc/group
```

---

### `/etc/shadow`

Stores protected password and password-aging information.

```text
/etc/shadow
```

---

## Troubleshooting Process

When troubleshooting Linux user and group problems, I learned to investigate the problem before making changes.

### Step 1 - Verify the User

```bash
grep '^halk:' /etc/passwd
```

This confirms whether the user account exists.

### Step 2 - Verify the Group

```bash
grep '^halk:' /etc/group
```

This confirms whether the group exists.

### Step 3 - Check Account Status

```bash
sudo passwd -S halk
```

This displays information about the user's password status.

### Step 4 - Check Running Processes

```bash
ps -u halk
```

This displays processes owned by the user.

### Step 5 - Terminate Processes if Required

```bash
sudo killall -u halk
```

This terminates processes owned by the specified user.

### Step 6 - Investigate Login Activity

```bash
lastlog2
```

```bash
last
```

```bash
w
```

These commands provide information about previous and current login activity.

---

## What I Learned

- How to verify that a Linux user exists using `/etc/passwd`.
- How to verify that a Linux group exists using `/etc/group`.
- How to identify a user's UID and GID.
- How to check processes owned by a specific user.
- How to terminate a user's running processes.
- How to check password and account status using `passwd -S`.
- How to use `lastlog2` to view the latest login information.
- How to use `last` to review previous login and logout sessions.
- How to use `w` to inspect currently active user sessions.
- How to troubleshoot Linux user and group problems in a logical order.
- How Linux administrators gather information before making account changes.

---

## Conclusion

This project helped me understand how Linux administrators troubleshoot user and group issues.

Instead of immediately modifying or deleting an account, I learned to first verify the user and group, check account status, inspect running processes, and investigate login activity.

These troubleshooting commands provide important information for diagnosing common Linux user and group problems.
