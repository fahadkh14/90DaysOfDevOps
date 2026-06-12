# Linux Practice: Processes and Services

## Process Checks

### Command 1


ps aux | head


Purpose: Display running processes.

Observation:

* Multiple system processes were running.
* Each process had a PID and resource usage information.

### Command 2


pgrep nginx


Purpose: Find Nginx process IDs.

Observation:

* Nginx process was running successfully.
* Multiple worker processes were visible.

---

## Service Checks

### Command 3


systemctl status nginx


Purpose: Check Nginx service status.

Observation:

* Nginx service was active (running).
* Service started successfully during boot.

### Command 4


systemctl list-units --type=service --state=running


Purpose: List all running services.

Observation:

* Multiple services such as nginx, systemd-journald, and ssh were running.

---

## Log Checks

### Command 5


journalctl -u nginx -n 20


Purpose: View the last 20 Nginx service log entries.

Observation:

* Nginx started successfully.
* No critical errors were found.

### Command 6


tail -n 20 /var/log/syslog


Purpose: View the latest system log entries.

Observation:

* Recent system events and service activities were displayed.

---

## Mini Troubleshooting Steps

### Scenario

Verify whether the Nginx service is working correctly.

### Steps

1. Check Nginx process:

pgrep nginx


2. Check Nginx service status:


systemctl status nginx


3. Check Nginx logs:


journalctl -u nginx -n 20


4. If service is stopped:


sudo systemctl restart nginx


5. Verify service again:


systemctl status nginx


### Result

Nginx service was running successfully and no major issues were detected.

---

## Summary

Today I practiced Linux process management, service inspection, and log analysis using commands such as ps, pgrep, systemctl, journalctl, and tail. I inspected the Nginx service and verified that it was running correctly. These commands are useful for troubleshooting services in real-world Linux environments.

