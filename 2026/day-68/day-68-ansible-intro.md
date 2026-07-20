# Day 68 - Introduction to Ansible and Inventory Setup

## Objective

Learn the fundamentals of Ansible, understand configuration management, create an inventory of managed servers, and execute ad-hoc commands using SSH without installing agents.

---

# What is Configuration Management?

Configuration Management is the process of maintaining systems in a desired and consistent state automatically.

Instead of configuring servers manually every time, configuration management tools automate tasks like:

- Installing software
- Managing users
- Configuring services
- Updating configuration files
- Starting and stopping services

### Why do we need it?

- Eliminates manual errors
- Ensures consistency across servers
- Saves time
- Makes infrastructure reproducible
- Easy to scale

---

# Ansible vs Chef vs Puppet vs Salt

| Feature | Ansible | Puppet | Chef | Salt |
|----------|----------|---------|-------|------|
| Agent Required | ❌ No | ✅ Yes | ✅ Yes | Optional |
| Language | YAML | Puppet DSL | Ruby DSL | YAML/Python |
| Learning Curve | Easy | Medium | Hard | Medium |
| Communication | SSH | Agent | Agent | SSH/Agent |
| Push/Pull | Push | Pull | Pull | Push/Pull |
| Best For | Automation & DevOps | Large Enterprises | Complex Automation | Fast Remote Execution |

---

# What does Agentless Mean?

Agentless means Ansible does not require any software installation on the managed servers.

It connects directly using SSH and executes tasks remotely.

Requirements:

- SSH access
- Python installed on managed nodes
- SSH private key

---

# Ansible Architecture

```
                +-----------------------+
                |     Control Node      |
                |  (Laptop / EC2 VM)    |
                |      Ansible          |
                +-----------+-----------+
                            |
                     SSH Connection
                            |
        -----------------------------------------
        |                 |                     |
+---------------+ +---------------+ +---------------+
| Web Server    | | App Server    | | DB Server     |
| Managed Node  | | Managed Node  | | Managed Node  |
+---------------+ +---------------+ +---------------+

Inventory
    |
inventory.ini

Modules
    |
command
copy
yum
apt
service
ping

Playbooks
    |
YAML files that define automation tasks
```

### Components

### Control Node

The machine where Ansible is installed.

Example:

- Laptop
- Ubuntu VM
- Jump Server

---

### Managed Nodes

Remote servers managed by Ansible.

Example:

- EC2 Web Server
- EC2 App Server
- EC2 Database Server

---

### Inventory

A file containing all managed servers grouped logically.

Example:

```ini
[web]
web-server ansible_host=54.xx.xx.xx

[app]
app-server ansible_host=13.xx.xx.xx

[db]
db-server ansible_host=18.xx.xx.xx
```

---

### Modules

Modules are small programs that perform tasks.

Examples:

- ping
- copy
- command
- shell
- yum
- apt
- service

---

### Playbooks

Playbooks are YAML files that automate multiple tasks.

Example:

```yaml
- hosts: web
  become: yes

  tasks:
    - name: Install Git
      yum:
        name: git
        state: present
```

---

# Lab Setup

### Cloud Provider

AWS EC2

### Provisioning Method

Terraform

### Instances

| Server | OS | Type |
|---------|----|------|
| Web | Ubuntu 22.04 | t2.micro |
| App | Ubuntu 22.04 | t2.micro |
| DB | Ubuntu 22.04 | t2.micro |

Security Group:

- SSH (22)

Authentication:

- PEM Key Pair

---

# Installing Ansible

Ubuntu

```bash
sudo apt update
sudo apt install ansible -y
```

Verify

```bash
ansible --version
```

Example

```text
ansible [core 2.18.x]

config file = None

python version = 3.x
```

---

# Why Install Ansible Only on Control Node?

Only the Control Node requires Ansible.

Managed nodes only need:

- SSH
- Python

Ansible copies temporary modules over SSH, executes them, and removes them afterward.

---

# Inventory File

`inventory.ini`

```ini
[web]
web-server ansible_host=<WEB_PUBLIC_IP>

[app]
app-server ansible_host=<APP_PUBLIC_IP>

[db]
db-server ansible_host=<DB_PUBLIC_IP>

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/my-key.pem
```

---

# Verify Connectivity

```bash
ansible all -i inventory.ini -m ping
```

Example Output

```text
web-server | SUCCESS =>

{
    "ping": "pong"
}

app-server | SUCCESS =>

{
    "ping": "pong"
}

db-server | SUCCESS =>

{
    "ping": "pong"
}
```

---

# Ad-Hoc Commands

## 1. Check Uptime

```bash
ansible all -m command -a "uptime"
```

---

## 2. Check Memory

```bash
ansible web -m command -a "free -h"
```

---

## 3. Check Disk Usage

```bash
ansible all -m command -a "df -h"
```

---

## 4. Install Git

Ubuntu

```bash
ansible web -m apt -a "name=git state=present" --become
```

Amazon Linux

```bash
ansible web -m yum -a "name=git state=present" --become
```

---

## 5. Copy File

Create

```bash
echo "Hello from Ansible" > hello.txt
```

Copy

```bash
ansible all -m copy -a "src=hello.txt dest=/tmp/hello.txt"
```

Verify

```bash
ansible all -m command -a "cat /tmp/hello.txt"
```

Output

```text
Hello from Ansible
```

---

# What Does --become Do?

`--become` allows Ansible to execute commands with elevated privileges (sudo/root).

Required for:

- Installing packages
- Starting services
- Editing system files
- Managing users

Example

```bash
ansible all -m apt -a "name=nginx state=present" --become
```

---

# Inventory Groups

```ini
[application:children]
web
app

[all_servers:children]
application
db
```

---

# Testing Groups

Application Servers

```bash
ansible application -m ping
```

Database

```bash
ansible db -m ping
```

All Servers

```bash
ansible all_servers -m ping
```

---

# Host Patterns

Web OR App

```bash
ansible 'web:app' -m ping
```

Everything Except DB

```bash
ansible 'all:!db' -m ping
```

---

# ansible.cfg

```ini
[defaults]

inventory = inventory.ini
host_key_checking = False
remote_user = ubuntu
private_key_file = ~/my-key.pem
```

Now run

```bash
ansible all -m ping
```

without specifying inventory.

---

# command vs shell Module

| command | shell |
|-----------|--------|
| Executes commands directly | Executes commands through a shell |
| Faster | Slightly slower |
| More secure | Less secure |
| Does not support pipes | Supports pipes |
| Does not support redirects | Supports redirects |
| Does not support variables | Supports shell variables |

Examples

Command

```bash
ansible all -m command -a "uptime"
```

Shell

```bash
ansible all -m shell -a "cat /etc/passwd | grep root"
```

---

# Learning Summary

Today I learned:

- Configuration Management fundamentals
- Why Ansible is agentless
- Ansible architecture
- Inventory creation
- SSH-based server management
- Running ad-hoc commands
- Inventory groups and patterns
- Difference between command and shell modules
- Using ansible.cfg for project configuration

---

# Screenshots to Include

- EC2 Instances
- ansible --version
- ansible all -m ping
- uptime output
- free -h output
- df -h output
- Git installation
- File copy verification

---

# Conclusion

Successfully installed Ansible on the Control Node, configured three EC2 managed nodes using an inventory file, verified SSH connectivity with the ping module, executed multiple ad-hoc commands, and learned how Ansible automates infrastructure management without requiring agents on remote servers.
