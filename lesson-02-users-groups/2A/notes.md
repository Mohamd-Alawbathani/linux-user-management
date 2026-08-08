# Project 2A - Linux User Management

In this project, I practiced basic Linux user management commands, including creating users, setting passwords, modifying user information, configuring password aging, verifying account information, and deleting users.

---

## 1. Creating a New User

I created a new Linux user named `Alxndr` and created a home directory for the account.

### Command

```bash
sudo useradd -m Alxndr
```

### Verification

```bash
id Alxndr
```

This command displays the user's UID, GID, and group membership.

### Screenshot

![Creating a New Linux User](2A-useradd-user-creation.png)

---

## 2. Setting a User Password

I set a password for the user using the `passwd` command.

### Command

```bash
sudo passwd alxndr
```

The message:

```text
passwd: password updated successfully
```

confirmed that the password was changed successfully.

### Password Test

I tested the password by switching to the user account:

```bash
su - alxndr
```

### Screenshot

![Setting a User Password](2A-passwd-user-password.png)

---

## 3. Verifying the User Entry

I checked the user's entry in `/etc/passwd`.

### Command

```bash
grep '^Alxndr:' /etc/passwd
```

The output shows information such as:

- Username
- UID
- GID
- User description
- Home directory
- Login shell

### Screenshot

![Verifying the User Entry](2A-passwd-entry-verification.png)

---

## 4. Modifying User Information

I modified the user information using the `usermod` command.

### Command

```bash
sudo usermod -c "Alxndr alee" Alxndr
```

The `-c` option changes the comment field for the user.

### Verification

```bash
grep '^Alxndr:' /etc/passwd
```

This confirmed that the user's information was updated successfully.

### Screenshot

![Modifying User Information](2A-usermod-user-update.png)

---

## 5. Configuring Password Aging and Account Expiration

I configured an expiration date for the user account.

### Account Expiration

```bash
sudo chage -E 2026-08-31 Alxndr
```

The account was configured to expire on:

```text
August 31, 2026
```

### Password Aging

I configured the password policy using:

```bash
sudo chage -m 30 -M 60 Alxndr
```

The policy was:

- Minimum password age: `30 days`
- Maximum password age: `60 days`
- Password warning period: `7 days`
- Account expiration date: `August 31, 2026`

### Verification

```bash
sudo chage -l Alxndr
```

### Screenshot

![Password Aging and Account Expiration](2A-chage-passwd-user.png)

---

## 6. Deleting the User

I deleted the user and removed the user's home directory.

### Command

```bash
sudo userdel -r Alxndr
```

### Verification

```bash
id Alxndr
```

After deletion, Linux displayed:

```text
id: ‘Alxndr’: no such user
```

This confirmed that the account was deleted successfully.

### Screenshot

![Deleting a Linux User](2A-userdel-user-deletion.png)

---

## Commands Practiced

```bash
sudo useradd -m Alxndr
id Alxndr

sudo passwd alxndr
su - alxndr

grep '^Alxndr:' /etc/passwd

sudo usermod -c "Alxndr alee" Alxndr
grep '^Alxndr:' /etc/passwd

sudo chage -E 2026-08-31 Alxndr
sudo chage -m 30 -M 60 Alxndr
sudo chage -l Alxndr

sudo userdel -r Alxndr
id Alxndr
```

---

## What I Learned

- How to create a Linux user.
- How to create a home directory for a new user.
- How to verify a user with `id`.
- How to set and test a user password.
- How to inspect a user's `/etc/passwd` entry.
- How to modify user information with `usermod`.
- How to configure password aging with `chage`.
- How to set an account expiration date.
- How to delete a user and verify that the account no longer exists.
