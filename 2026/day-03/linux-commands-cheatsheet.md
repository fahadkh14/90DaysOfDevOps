# Linux Commands Cheat Sheet

## Process Management

### ps aux

View all running processes.

### top

Display real-time process and system usage.

### htop

Interactive process viewer.

### kill PID

Terminate a process by PID.

### kill -9 PID

Forcefully terminate a process.

### pkill process_name

Kill process by name.

### pgrep process_name

Find process ID by name.

### jobs

List background jobs.

### bg

Run a job in the background.

### fg

Bring a background job to the foreground.

---

## File System Commands

### pwd

Show current directory.

### ls -la

List files and directories with details.

### cd directory

Change directory.

### mkdir directory

Create a new directory.

### rm -r directory

Remove a directory and its contents.

### cp source destination

Copy files or directories.

### mv source destination

Move or rename files.

### touch file.txt

Create an empty file.

### cat file.txt

Display file contents.

### find /path -name "filename"

Search for files.

---

## Disk Usage Commands

### df -h

Show disk space usage.

### du -sh directory

Show directory size.

### free -h

Display memory usage.

---

## Networking Commands

### ip addr

Display IP addresses and network interfaces.

### ping google.com

Test network connectivity.

### curl https://example.com

Fetch data from a URL.

### dig google.com

Query DNS information.

### nslookup google.com

Lookup DNS records.

### ss -tulnp

Display listening ports and network connections.

---

## Log & Service Commands

### journalctl

View system logs.

### journalctl -u nginx

View logs for a specific service.

### systemctl status nginx

Check service status.

### systemctl start nginx

Start a service.

### systemctl stop nginx

Stop a service.

### systemctl restart nginx

Restart a service.

### systemctl enable nginx

Enable service at boot.

---

## Summary

These commands are commonly used for process management, file system navigation, networking troubleshooting, disk usage analysis, and service management. They form the foundation of daily Linux and DevOps operations.

