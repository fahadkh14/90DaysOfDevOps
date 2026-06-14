# Day 09 - Linux User & Group Management Challenge

## Objective

Practice Linux user management, group management, and shared directory permissions through hands-on exercises.

---

# Task 1: Create Users

## Create Users

```bash
sudo useradd -m tokyo
sudo passwd tokyo

sudo useradd -m berlin
sudo passwd berlin

sudo useradd -m professor
sudo passwd professor
```

## Verify Users

```bash
grep "tokyo\|berlin\|professor" /etc/passwd
ls -l /home
```

### Observation

Successfully created users with their home directories.

---

# Task 2: Create Groups

## Create Groups

```bash
sudo groupadd developers
sudo groupadd admins
```

## Verify Groups

```bash
grep "developers\|admins" /etc/group
```

### Observation

Groups developers and admins were created successfully.

---

# Task 3: Assign Users to Groups

## Assign Users

```bash
sudo usermod -aG developers tokyo

sudo usermod -aG developers,admins berlin

sudo usermod -aG admins professor
```

## Verify Group Membership

```bash
groups tokyo
groups berlin
groups professor
```

### Observation

| User      | Groups             |
| --------- | ------------------ |
| tokyo     | developers         |
| berlin    | developers, admins |
| professor | admins             |

---

# Task 4: Shared Directory

## Create Directory

```bash
sudo mkdir -p /opt/dev-project
```

## Set Group Owner

```bash
sudo chgrp developers /opt/dev-project
```

## Set Permissions

```bash
sudo chmod 775 /opt/dev-project
```

## Verify Permissions

```bash
ls -ld /opt/dev-project
```

Expected Output:

```text
drwxrwxr-x
```

## Test Access

```bash
sudo -u tokyo touch /opt/dev-project/tokyo.txt

sudo -u berlin touch /opt/dev-project/berlin.txt
```

## Verify Files

```bash
ls -l /opt/dev-project
```

### Observation

Both users were able to create files successfully.

---

# Task 5: Team Workspace

## Create User

```bash
sudo useradd -m nairobi
sudo passwd nairobi
```

## Create Group

```bash
sudo groupadd project-team
```

## Add Members

```bash
sudo usermod -aG project-team nairobi

sudo usermod -aG project-team tokyo
```

## Create Shared Workspace

```bash
sudo mkdir -p /opt/team-workspace
```

## Assign Group

```bash
sudo chgrp project-team /opt/team-workspace
```

## Set Permissions

```bash
sudo chmod 775 /opt/team-workspace
```

## Verify Permissions

```bash
ls -ld /opt/team-workspace
```

## Test Access

```bash
sudo -u nairobi touch /opt/team-workspace/nairobi.txt
```

## Verify Files

```bash
ls -l /opt/team-workspace
```

### Observation

Nairobi successfully created files inside the shared workspace.

---

# Users & Groups Created

## Users

* tokyo
* berlin
* professor
* nairobi

## Groups

* developers
* admins
* project-team

---

# Group Assignments

| User      | Groups                   |
| --------- | ------------------------ |
| tokyo     | developers, project-team |
| berlin    | developers, admins       |
| professor | admins                   |
| nairobi   | project-team             |

---

# Directories Created

| Directory           | Group Owner  | Permissions |
| ------------------- | ------------ | ----------- |
| /opt/dev-project    | developers   | 775         |
| /opt/team-workspace | project-team | 775         |

---

# Screenshots Attached

### Screenshot 1 - Users Created

```bash
grep "tokyo\|berlin\|" /etc/passwd

File Name: /home/fahad/Pictures/Screenshots/Screenshot from 2026-06-15 00-38-06.png
```

File Name: //home/fahad/Pictures/Screenshots/Screenshot%20from%202026-06-15%2000-43-35.png

```text
users-created.png
```

---

### Screenshot 2 - Groups Created

```bash
 grep "developers\|admins" /etc/group

```


```text
groups-created.png
```


# Commands Used

```bash
useradd -m
passwd
groupadd
usermod -aG
groups
grep
mkdir
chgrp
chmod
touch
ls -l
ls -ld
```

---

# What I Learned

1. How to create Linux users and home directories.
2. How to create and manage Linux groups.
3. How to assign users to multiple groups.
4. How shared directory permissions work using groups.
5. How chmod and chgrp are used for access management.
6. How DevOps teams manage collaboration through user groups.

---

# Challenges Faced

### Challenge

Users could not create files in the shared directory initially.

### Solution

Verified group membership and corrected directory permissions using:

```bash
sudo chmod 775 /opt/dev-project
sudo chmod 775 /opt/team-workspace
```

---

# Conclusion

Successfully completed Linux User & Group Management Challenge by creating users, groups, assigning memberships, configuring shared directories, testing permissions, and verifying access using real Linux commands.

