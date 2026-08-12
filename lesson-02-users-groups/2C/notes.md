# Project 2C - Privilege Escalation and Sudo Management

In this project, I practiced Linux privilege escalation and administrative access using `sudo`, `su`, `visudo`, and `sudoedit`.

The goal of this project was to understand how Linux controls elevated privileges and how to apply the principle of least privilege.

---

## 1. Listing Sudo Permissions

I checked which commands the current user is allowed to run with `sudo`.

### Command

```bash
sudo -l
```

This command displays the sudo permissions assigned to the current user.

### Screenshot

![Listing Sudo Permissions](2C-sudo-list-permissions.png)

---

## 2. Comparing `su` and `su -`

I practiced switching to the root account using two different methods.

### Commands

```bash
su root
```

and:

```bash
su - root
```

### Difference

`su root` switches to the root user while keeping much of the current user's environment.

`su - root` starts a full login shell for root and loads the root user's environment.

I also used:

```bash
pwd
```

to compare the working directory and environment.

### Screenshot

![Comparing su and su login shell](2C-su-vs-su-login-shell.png)

---

## 3. Safely Editing Sudoers with `visudo`

I opened the sudoers configuration using:

```bash
sudo visudo
```

`visudo` is the recommended way to edit sudo rules because it checks the sudoers file syntax before saving changes.

This helps reduce the risk of breaking sudo access because of a configuration error.

### Screenshot

![Editing sudoers safely with visudo](2C-visudo-safe-edit.png)

---

## 4. Adding a User to the Sudo Group

I added the user `Alxndr` to the `sudo` group.

### Command

```bash
sudo usermod -aG sudo Alxndr
```

### Verification

```bash
groups Alxndr
```

This confirmed that the user was a member of the `sudo` group.

### Screenshot

![Adding a User to the Sudo Group](2C-add-user-to-sudo-group.png)

---

## 5. Verifying Sudo Access

I switched to the user account and tested elevated privileges.

### Commands

```bash
su - Alxndr
sudo whoami
```

If sudo access is configured correctly, the output is:

```text
root
```

This confirms that the user can execute commands with root privileges through `sudo`.

### Screenshot

![Verifying Sudo Access](2C-sudo-whoami-verification.png)

---

## 6. Testing Limited Sudo Access

I configured the user `Alxndr` to use only a specific privileged command.

Inside `visudo`, I used the following rule:

```text
Alxndr ALL=(ALL) /usr/bin/apt
```

I then removed the user from the full `sudo` group:

```bash
sudo deluser Alxndr sudo
```

### Verification

I checked the remaining sudo permissions using:

```bash
sudo -l -U Alxndr
```

The allowed command was:

```text
(ALL) /usr/bin/apt
```

I switched to the user:

```bash
su - Alxndr
```

The allowed command worked:

```bash
sudo apt update
```

But an unauthorized command was denied:

```bash
sudo useradd testuser
```

This demonstrated the **Principle of Least Privilege**, where a user receives only the permissions required for a specific task.

### Screenshot

![Testing Limited Sudo Access](2C-limited-sudo-test.png)

---

## 7. Removing Full Sudo Access

I removed `Alxndr` from the `sudo` group.

### Command

```bash
sudo deluser Alxndr sudo
```

### Verification

```bash
groups Alxndr
```

The `sudo` group was no longer listed for the user.

I also checked the user's sudo permissions using:

```bash
sudo -l -U Alxndr
```

### Screenshot

![Removing Full Sudo Access](2C-remove-sudo-access.png)

---

## 8. Editing a Protected File with `sudoedit`

I used `sudoedit` to safely open the protected `/etc/hosts` file.

### Command

```bash
sudoedit /etc/hosts
```

`sudoedit` allows an authorized user to edit a protected system file without running the entire text editor directly as root.

This is safer than using:

```bash
sudo nano /etc/hosts
```

because the editor itself does not need to run with full root privileges.

### Screenshot

![Editing the Hosts File with sudoedit](2C-sudoedit-hosts.png)

---

## Commands Practiced

```bash
sudo -l

su root
su - root
pwd

sudo visudo

sudo usermod -aG sudo Alxndr
groups Alxndr

su - Alxndr
sudo whoami

sudo -l -U Alxndr

sudo deluser Alxndr sudo

sudo apt update
sudo useradd testuser

sudoedit /etc/hosts
```

---

## What I Learned

* How to check sudo permissions using `sudo -l`.
* The difference between `su` and `su -`.
* How to safely edit sudoers using `visudo`.
* How to add a user to the `sudo` group.
* How to verify elevated privileges using `sudo whoami`.
* How to create and test limited sudo access.
* How to apply the principle of least privilege.
* How to remove full sudo access from a user.
* How to edit protected files safely using `sudoedit`.
* How Linux controls administrative privileges.
