# Day 07 - Linux File System Hierarchy & Scenario-Based Practice

# Part 1: Linux File System Hierarchy

## 1. / (Root Directory)

* Linux file system ka starting point.
* Sabhi files aur directories isi ke andar hoti hain.

**Command**


ls -l /


**I would use this when:** Mujhe Linux file system ka overall structure dekhna ho.

---

## 2. /home

* Normal users ke home directories yahan store hote hain.
* Har user ka apna folder hota hai.

**Command**


ls -l /home


**I would use this when:** User files aur personal data access karna ho.

---

## 3. /root

* Root user ka home directory.
* Sirf root user ko full access hota hai.

**Command**


ls -l /root


**I would use this when:** Root user ke configuration ya files check karni ho.

---

## 4. /etc

* System configuration files store hoti hain.
* Nginx, SSH, hostname, network settings yahin milti hain.

**Command**


ls -l /etc


**Example File**


cat /etc/hostname


**I would use this when:** Kisi service ki configuration change karni ho.

---

## 5. /var/log

* System aur application logs yahan store hote hain.
* DevOps troubleshooting ka sabse important location.

**Command**


ls -l /var/log


**Find Largest Logs**


du -sh /var/log/* 2>/dev/null | sort -h | tail -5


**I would use this when:** Error ya service failure troubleshoot karna ho.

---

## 6. /tmp

* Temporary files ke liye use hota hai.
* Reboot ke baad files remove ho sakti hain.

**Command**


ls -l /tmp


**I would use this when:** Temporary scripts ya testing files create karni ho.

---

## 7. /bin

* Essential Linux commands store hoti hain.
* Example: ls, cp, mv, cat

**Command**


ls -l /bin


**I would use this when:** Basic system commands locate karni ho.

---

## 8. /usr/bin

* Additional user commands aur applications.
* Large number of executable files.

**Command**


ls -l /usr/bin | head


**I would use this when:** Installed command binaries check karni ho.

---

## 9. /opt

* Third-party applications install hoti hain.

**Command**


ls -l /opt


**I would use this when:** Custom software installations manage karni ho.

---

## Home Directory Check


ls -la ~

Observation:

* Home directory me hidden files (.bashrc, .profile) aur personal folders mile.

---

# Part 2: Scenario-Based Practice

## Scenario 1: Service Not Starting

### Step 1


systemctl status myapp


**Why:** Service running hai, failed hai ya stopped hai check karne ke liye.

### Step 2

```bash
journalctl -u myapp -n 50
```

**Why:** Recent logs aur errors dekhne ke liye.

### Step 3

```bash
systemctl is-enabled myapp
```

**Why:** Check karne ke liye ki reboot ke baad service automatically start hogi ya nahi.

### Step 4

```bash
systemctl list-units --type=service
```

**Why:** Confirm karne ke liye ki service system me available hai.

### What I Learned

Status → Logs → Boot Configuration → Service Verification ka flow follow karna chahiye.

---

## Scenario 2: High CPU Usage

### Step 1

```bash
top
```

**Why:** Live CPU usage monitor karne ke liye.

### Step 2

```bash
ps aux --sort=-%cpu | head -10
```

**Why:** Highest CPU consuming processes identify karne ke liye.

### Step 3

```bash
ps -p <PID> -f
```

**Why:** Specific process details dekhne ke liye.

### Step 4

```bash
htop
```

**Why:** Better interactive process monitoring ke liye.

### What I Learned

Pehle CPU usage identify karo, phir PID aur process details investigate karo.

---

## Scenario 3: Finding Docker Service Logs

### Step 1

```bash
systemctl status docker
```

**Why:** Docker service ki health check karne ke liye.

### Step 2

```bash
journalctl -u docker -n 50
```

**Why:** Recent logs aur errors dekhne ke liye.

### Step 3

```bash
journalctl -u docker -f
```

**Why:** Real-time logs monitor karne ke liye.

### What I Learned

Systemd services ke logs journalctl ke through access kiye jaate hain.

---

## Scenario 4: File Permission Issue

### Step 1

```bash
ls -l /home/user/backup.sh
```

**Why:** Current permissions check karne ke liye.

### Step 2

```bash
chmod +x /home/user/backup.sh
```

**Why:** Execute permission add karne ke liye.

### Step 3

```bash
ls -l /home/user/backup.sh
```

**Why:** Verify karne ke liye ki execute permission add hui ya nahi.

### Step 4

```bash
./backup.sh
```

**Why:** Script successfully run ho rahi hai ya nahi check karne ke liye.

### What I Learned

Script execute karne ke liye execute (x) permission required hoti hai.

---

# Summary

* Linux file system hierarchy samjhi.
* Important directories jaise /etc, /var/log aur /home ka purpose seekha.
* Service troubleshooting, CPU analysis, log investigation aur file permission issues solve karne ka process practice kiya.
* DevOps troubleshooting ke liye structured approach develop ki.

