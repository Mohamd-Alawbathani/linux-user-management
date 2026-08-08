# Project 2A - Linux User Management

In this project, I practiced creating, modifying, securing, verifying, and deleting Linux user accounts.

---

## 1. Creating a New User

I created a new Linux user named `Alxndr` with a home directory.

### Command

```bash
sudo useradd -m Alxndr
```

### Verification

```bash
id Alxndr
```

### Screenshot

![Creating a New Linux User](2A-useradd-user-creation.png)

---

## 2. Setting a User Password

I set a password for the user using the `passwd` command.

### Command

```bash
sudo passwd alxndr
```

### Verification

The following message confirmed that the password was updated successfully:

```text
passwd: password updated successfully
```

I also tested the password by switching to the user account:

```bash
su - alxndr
```

### Screenshot

![Setting a User Password](2A-passwd-user-password.png)

---

## 3. Viewing Password Status

I checked the current password status for the user.

### Command

```bash
sudo passwd -S Alxndr
```

This command displays information about the current password status.

### Screenshot

![Viewing Password Status](2A-user-password-status.png)

---

## 4. Locking the User Account

I locked the user account using `usermod`.

### Command

```bash
sudo usermod -L Alxndr
```

### Verification

```bash
sudo passwd -S Alxndr
```

### Screenshot

![Locking the User Account](2A-user-lock-account.png)

---

## 5. Unlocking the User Account

I unlocked the user account.

### Command

```bash
sudo usermod -U Alxndr
```

### Verification

```bash
sudo passwd -S Alxndr
```

### Screenshot

![Unlocking the User Account](2A-user-unlock-account.png)

---

## 6. Modifying User Information

I changed the comment field for the user account.

### Command

```bash
sudo usermod -c "Alxndr alee" Alxndr
```

### Verification

```bash
grep '^Alxndr:' /etc/passwd
```

### Screenshot

![Modifying User Information](2A-usermod-user-update.png)

---

## 7. Changing the User Shell

I changed the user's login shell to Bash.

### Command

```bash
sudo usermod -s /bin/bash Alxndr
```

### Verification

```bash
getent passwd Alxndr
```

### Screenshot

![Changing the User Shell](2A-user-change-shell.png)

---

## 8. Viewing User ID Information

I checked the user's UID, GID, and group memberships.

### Command

```bash
id Alxndr
```

### Screenshot

![Viewing User ID Information](2A-user-id-verification.png)

---

## 9. Viewing the User Entry in `/etc/passwd`

I checked the user's account entry inside `/etc/passwd`.

### Command

```bash
grep '^Alxndr:' /etc/passwd
```

The output includes information such as:

- Username
- UID
- GID
- User description
- Home directory
- Login shell

### Screenshot

![Viewing the Passwd Entry](2A-passwd-entry-verification.png)

---

## 10. Configuring Account Expiration

I configured the account to expire on August 31, 2026.

### Command

```bash
sudo chage -E 2026-08-31 Alxndr
```

### Verification

```bash
sudo chage -l Alxndr
```

### Screenshot

![Configuring Account Expiration](2A-user-expiration-verification.png)

---

## 11. Configuring Password Aging

I configured minimum and maximum password ages.

### Command

```bash
sudo chage -m 30 -M 60 Alxndr
```

The password policy was configured as follows:

- Minimum password age: `30 days`
- Maximum password age: `60 days`
- Password warning period: `7 days`
- Account expiration date: `August 31, 2026`

### Verification

```bash
sudo chage -l Alxndr
```

### Screenshot

![Configuring Password Aging](2A-chage-passwd-user.png)

---

## 12. Changing the Home Directory

I changed the user's home directory.

### Command

```bash
sudo usermod -d /home/Alxndr-new -m Alxndr
```

The `-m` option moves the contents of the old home directory to the new directory.

### Verification

```bash
getent passwd Alxndr
```

### Screenshot

![Changing the Home Directory](2A-user-change-home-directory.png)

---

## 13. Verifying the User Account

I verified that the user account still existed.

### Command

```bash
getent passwd Alxndr
```

### Screenshot

![Verifying the User Account](2A-user-account-verification.png)

---

## 14. Deleting the User

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

This confirmed that the user account was deleted successfully.

### Screenshot

![Deleting a Linux User](2A-userdel-user-deletion.png)

---

## Commands Practiced

```bash
sudo useradd -m Alxndr
id Alxndr

sudo passwd alxndr
su - alxndr

sudo passwd -S Alxndr

sudo usermod -L Alxndr
sudo passwd -S Alxndr

sudo usermod -U Alxndr
sudo passwd -S Alxndr

sudo usermod -c "Alxndr alee" Alxndr
grep '^Alxndr:' /etc/passwd

sudo usermod -s /bin/bash Alxndr
getent passwd Alxndr

id Alxndr

grep '^Alxndr:' /etc/passwd

sudo chage -E 2026-08-31 Alxndr
sudo chage -m 30 -M 60 Alxndr
sudo chage -l Alxndr

sudo usermod -d /home/Alxndr-new -m Alxndr
getent passwd Alxndr

getent passwd Alxndr

sudo userdel -r Alxndr
id Alxndr
```

---

## What I Learned

- How to create a Linux user.
- How to create a home directory for a user.
- How to set and test a password.
- How to check password status.
- How to lock and unlock user accounts.
- How to modify user information.
- How to change a user's login shell.
- How to check UID, GID, and group memberships.
- How to inspect `/etc/passwd`.
- How to configure account expiration.
- How to configure password aging.
- How to change a user's home directory.
- How to verify a user account.
- How to delete a user and verify that the account no longer exists.
