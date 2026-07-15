
# Lesson 02 - Users and Groups

In this lesson, I practiced creating, modifying, securing, and deleting Linux user accounts.

---

## 1. Creating a New User

I created a new user named `Alxndr` and gave the account a home directory.

### Command

```bash
sudo useradd -m Alxndr
```

The `-m` option creates a home directory for the new user.

### Verification

I used the following command to confirm that the account was created successfully:

```bash
id Alxndr
```

The output displayed the user's UID, GID, and group membership.

### Screenshot

![Creating a Linux User](screenshots/2A/2A-useradd-user-creation.png)

---

## 2. Setting and Testing the User Password

I created a password for the user `alxndr` using the `passwd` command.

### Command

```bash
sudo passwd alxndr
```

The following message confirmed that the password was updated successfully:

```text
passwd: password updated successfully
```

I then tested the password by switching to the user account:

```bash
su - alxndr
```

After successfully logging in, I returned to my original account using:

```bash
exit
```

### Screenshot

![Setting and Testing a User Password](screenshots/2A/2A-passwd-user-password.png)

---

## 3. Modifying User Information

I modified the account information for the user `Alxndr` using the `usermod` command.

### Command

```bash
sudo usermod -c "Alxndr alee" Alxndr
```

The `-c` option changes the comment field, which is commonly used to store the user's full name or description.

### Verification

I checked the user's entry inside `/etc/passwd` using:

```bash
grep '^Alxndr:' /etc/passwd
```

The output confirmed that the comment field was changed to:

```text
Alxndr alee
```

### Screenshot

![Modifying a Linux User](screenshots/2A/2A-usermod-user-update.png)

---

## 4. Configuring Account and Password Expiration

I configured an expiration date for the account:

```bash
sudo chage -E 2026-08-31 Alxndr
```

This means that the account will expire on:

```text
August 31, 2026
```

I then configured the password-aging policy:

```bash
sudo chage -m 30 -M 60 Alxndr
```

The password policy was configured as follows:

- Minimum password age: `30 days`
- Maximum password age: `60 days`
- Password warning period: `7 days`
- Account expiration date: `August 31, 2026`

### Verification

I displayed the complete account-aging information using:

```bash
sudo chage -l Alxndr
```

The output confirmed that:

```text
Password expires: Sep 13, 2026
Account expires: Aug 31, 2026
Minimum number of days between password change: 30
Maximum number of days between password change: 60
Number of days of warning before password expires: 7
```

### Screenshot

![Configuring Account and Password Expiration](screenshots/2A/2A-chage-passwd-user.png)

---

## 5. Deleting the User

I deleted the user `Alxndr` and attempted to remove the user's home directory and files.

### Command

```bash
sudo userdel -r Alxndr
```

The `-r` option removes the user's home directory and mail spool.

Linux displayed this message:

```text
userdel: Alxndr mail spool (/var/mail/Alxndr) not found
```

This only means that the user did not have a mail spool file.

### Verification

I confirmed that the user was deleted using:

```bash
id Alxndr
```

Linux displayed:

```text
id: ‘Alxndr’: no such user
```

This confirmed that the account was deleted successfully.

### Screenshot

![Deleting a Linux User](screenshots/2A/2A-userdel-user-deletion.png)

---

## Commands Practiced

```bash
sudo useradd -m Alxndr
id Alxndr
sudo passwd alxndr
su - alxndr
exit
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

In this lesson, I learned how to:

- Create a Linux user with a home directory.
- Verify a user account using `id`.
- Set and test a user's password.
- Modify user information using `usermod`.
- Configure account expiration using `chage`.
- Configure minimum and maximum password ages.
- Verify password-aging settings.
- Delete a user and verify that the account no longer exists.
