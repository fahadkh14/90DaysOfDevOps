# Linux Architecture, Processes, and systemd

## 1. Core Components of Linux

### Kernel

* Kernel is the core part of the Linux operating system.
* It manages hardware resources such as CPU, memory, disks, and devices.
* It acts as a bridge between hardware and software.

### User Space

* User space is where applications and users work.
* Programs like browsers, editors, and shell commands run in user space.
* User space communicates with the kernel through system calls.

### Init / systemd

* systemd is the first process started by the Linux kernel.
* It has Process ID (PID) 1.
* It manages services, startup processes, and system states.

---

## 2. Linux Processes

### What is a Process?

* A process is a running instance of a program.
* Every process has a unique Process ID (PID).

### Process States

#### Running (R)

* The process is currently using the CPU.

#### Sleeping (S)

* The process is waiting for an event or resource.

#### Stopped (T)

* The process has been paused or stopped.

#### Zombie (Z)

* The process has finished execution but still exists in the process table until its parent collects the exit status.

---

## 3. Process Management

Common process management tasks:

* Start a process
* Stop a process
* Monitor CPU and memory usage
* Check process status

Useful Commands:

### ps

Displays running processes.

Example:


ps aux


### top

Shows real-time system and process information.

Example:


top


### kill

Terminates a process using its PID.

Example:


kill 1234


---

## 4. What is systemd?

* systemd is the service manager used in most Linux distributions.
* It starts and manages system services.
* It handles service restarts and boot processes.
* It provides centralized logging through journalctl.

### Useful systemd Commands

Check service status:


systemctl status nginx

Start a service:


systemctl start nginx


Stop a service:


systemctl stop nginx


Enable service at boot:


systemctl enable nginx


View logs:


journalctl -u nginx


---

## 5. Five Linux Commands I Would Use Daily

1. `ps aux` – View running processes.
2. `top` – Monitor CPU and memory usage.
3. `systemctl status <service>` – Check service status.
4. `journalctl` – View system logs.
5. `kill <PID>` – Stop a process.

---

## Summary

Linux consists of the Kernel, User Space, and systemd. Processes are running programs managed by the kernel. systemd is responsible for starting and managing services and plays a key role in system administration and troubleshooting.

