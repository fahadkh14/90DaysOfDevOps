# Linux Troubleshooting Runbook

## Target Service

**Nginx Web Server (nginx.service)**

---

## 1. Environment Basics

### Command 1


uname -a


**Observation:** System kernel version aur architecture verify kiya.

### Command 2


cat /etc/os-release


**Observation:** Ubuntu version aur distribution details confirm ki.

---

## 2. Filesystem Sanity Check

### Command 3


mkdir /tmp/runbook-demo


**Observation:** Temporary troubleshooting directory create hui.

### Command 4


cp /etc/hosts /tmp/runbook-demo/hosts-copy
ls -l /tmp/runbook-demo


**Observation:** File successfully copy hui aur permissions verify kiye.

---

## 3. CPU & Memory Snapshot

### Command 5


top


**Observation:** Nginx process CPU usage normal thi aur system responsive tha.

### Command 6


free -h


**Observation:** Available memory sufficient thi, memory pressure observe nahi hua.

---

## 4. Disk & IO Snapshot

### Command 7


df -h


**Observation:** Root filesystem me adequate free space available tha.

### Command 8


du -sh /var/log


**Observation:** Log directory ka size normal tha aur disk usage controlled thi.

---

## 5. Network Snapshot

### Command 9


ss -tulpn | grep nginx


**Observation:** Nginx port 80 par listening state me tha.

### Command 10


curl -I http://localhost


**Observation:** HTTP response `200 OK` receive hua, website accessible thi.

---

## 6. Logs Review

### Command 11


journalctl -u nginx -n 50


**Observation:** Recent Nginx service logs review kiye, koi critical error nahi mila.

### Command 12


tail -n 50 /var/log/nginx/error.log


**Observation:** Error log me recent failures ya configuration issues nahi mile.

---

## Quick Findings

* Nginx service active aur running thi.
* CPU aur memory utilization normal tha.
* Disk space sufficient tha.
* Port 80 successfully listening tha.
* Logs me koi critical error observe nahi hua.

---

## If This Worsens

### 1. Service Status Check


sudo systemctl status nginx


### 2. Configuration Test


sudo nginx -t


### 3. Restart Service


sudo systemctl restart nginx


### 4. Live Log Monitoring


sudo journalctl -u nginx -f


### 5. Process-Level Investigation

ps aux | grep nginx


