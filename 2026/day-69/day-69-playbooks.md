# Day 69 – Ansible Playbooks and Modules

## Objective

Today I learned how to automate server configuration using Ansible Playbooks. I created multiple playbooks to install packages, manage services, copy files, use handlers, and understand idempotency.

---

# Task 1 – Install Nginx Playbook

## install-nginx.yml

```yaml
---
- name: Install and Start Nginx
  hosts: web
  become: true

  tasks:

    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start and Enable Nginx
      service:
        name: nginx
        state: started
        enabled: true

    - name: Create Custom Index Page
      copy:
        content: |
          <h1>Deployed by Ansible - Day 69</h1>
        dest: /var/www/html/index.html
```

### Run

```bash
ansible-playbook install-nginx.yml
```

### Result

* Nginx installed successfully.
* Service started and enabled.
* Custom index page deployed.
* Running the playbook again showed **ok** instead of **changed**, demonstrating **idempotency**.

---

# Task 2 – Playbook Structure

```yaml
---
- name: Play Name
  hosts: web
  become: true

  tasks:

    - name: Task Name
      apt:
        name: nginx
        state: present
```

### Explanation

| Component | Description                                    |
| --------- | ---------------------------------------------- |
| Play      | Targets a group of hosts                       |
| Hosts     | Inventory group where play runs                |
| Become    | Executes tasks with sudo                       |
| Task      | A single unit of work                          |
| Module    | Performs the action (apt, service, copy, etc.) |

### Answers

### 1. Difference between Play and Task

A Play targets one or more hosts, whereas a Task performs a single action using an Ansible module.

### 2. Can a playbook contain multiple plays?

Yes. One playbook can contain multiple plays targeting different host groups.

### 3. become: true

At the play level, all tasks run with sudo privileges.

At the task level, only that task runs with sudo.

### 4. What happens if a task fails?

By default, Ansible stops executing the remaining tasks for that host.

---

# Task 3 – Essential Modules

## apt

```yaml
- apt:
    name:
      - git
      - curl
      - wget
      - tree
    state: present
```

Installs packages.

---

## service

```yaml
- service:
    name: nginx
    state: started
    enabled: true
```

Starts and enables services.

---

## copy

```yaml
- copy:
    src: files/app.conf
    dest: /etc/app.conf
```

Copies files from the control node to managed nodes.

---

## file

```yaml
- file:
    path: /opt/myapp
    state: directory
    mode: '0755'
```

Creates directories and manages permissions.

---

## command

```yaml
- command: df -h
```

Runs commands **without shell features**.

---

## shell

```yaml
- shell: ps aux | wc -l
```

Runs commands **with shell features** such as pipes and redirects.

---

## lineinfile

```yaml
- lineinfile:
    path: /etc/environment
    line: "TZ=Asia/Kolkata"
```

Adds or updates a single line inside a file.

---

# command vs shell

| command                             | shell                                     |
| ----------------------------------- | ----------------------------------------- |
| Does not support pipes or redirects | Supports pipes, redirects, variables      |
| More secure                         | More flexible                             |
| Preferred whenever possible         | Use only when shell features are required |

---

# Task 4 – Handlers

## nginx-config.yml

```yaml
---
- name: Configure Nginx
  hosts: web
  become: true

  tasks:

    - name: Deploy nginx config
      copy:
        src: files/nginx.conf
        dest: /etc/nginx/nginx.conf
      notify: Restart Nginx

  handlers:

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

### Handler Behavior

**First Run**

* Configuration changed.
* Handler executed.
* Nginx restarted.

**Second Run**

* No changes detected.
* Handler did not execute.

This demonstrates that handlers only run when notified.

---

# Task 5 – Useful Commands

### Dry Run

```bash
ansible-playbook install-nginx.yml --check
```

### Diff Mode

```bash
ansible-playbook nginx-config.yml --check --diff
```

### Verbose

```bash
ansible-playbook install-nginx.yml -v
```

### More Verbose

```bash
ansible-playbook install-nginx.yml -vv
```

### Connection Debugging

```bash
ansible-playbook install-nginx.yml -vvv
```

### Limit to One Host

```bash
ansible-playbook install-nginx.yml --limit web-server
```

### List Hosts

```bash
ansible-playbook install-nginx.yml --list-hosts
```

### List Tasks

```bash
ansible-playbook install-nginx.yml --list-tasks
```

---

# Why use --check --diff?

`--check` performs a dry run and shows what changes would occur without modifying the system.

`--diff` displays the exact differences in files before applying them.

Using both together helps safely validate changes before deploying to production.

---

# Task 6 – Multiple Plays

A single playbook can contain multiple plays.

Example:

* Web servers → Install Nginx
* App servers → Create application directory
* Database servers → Install MySQL Client

Each play targets only its own inventory group.

---

# Screenshots

Add the following screenshots:

* Playbook first execution
* Playbook second execution (showing **ok**)
* Handler first execution
* Handler second execution
* Browser output
* `--check --diff` output

---

# Conclusion

Today I learned how to write Ansible Playbooks, use commonly used modules, manage services, configure files, implement handlers, and understand idempotency. Running the same playbook multiple times results in no unnecessary changes, making Ansible reliable and predictable for infrastructure automation.
