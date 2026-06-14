# Day 08 - Cloud Server Setup: Docker, Nginx & Web Deployment

## Objective

Deploy a cloud server, connect via SSH, install Docker and Nginx, configure security groups, and access the website from the internet.

---

# Part 1: Launch Cloud Instance & SSH Access

## Launch EC2 Instance

* Cloud Provider: AWS EC2
* OS: Ubuntu Server 24.04 LTS
* Instance Type: t2.micro (Free Tier)
* Security Group:

  * SSH (22) → My IP
  * HTTP (80) → Anywhere (0.0.0.0/0)

## Connect via SSH

```bash
ssh -i my-key.pem ubuntu@<PUBLIC-IP>
```

### Verification

```bash
whoami
hostname
```

**Observation:**
Successfully connected to the cloud server using SSH.

---

# Part 2: Install Docker & Nginx

## Update System

```bash
sudo apt update && sudo apt upgrade -y
```

## Install Docker

```bash
sudo apt install docker.io -y
```

### Verify Docker

```bash
sudo systemctl status docker
docker --version
```

**Observation:**
Docker installed and running successfully.

---

## Install Nginx

```bash
sudo apt install nginx -y
```

### Verify Nginx

```bash
sudo systemctl status nginx
```

```bash
curl http://localhost
```

**Observation:**
Nginx service started successfully and returned the default HTML page.

---

# Part 3: Security Group Configuration

## Open HTTP Port

Configured Security Group:

| Port | Protocol | Source    |
| ---- | -------- | --------- |
| 22   | TCP      | My IP     |
| 80   | TCP      | 0.0.0.0/0 |

## Test Web Access

Browser URL:

```text
http://<PUBLIC-IP>
```

**Result:**
Nginx Welcome Page displayed successfully.

---

# Part 4: Extract Nginx Logs

## View Access Logs

```bash
sudo tail -50 /var/log/nginx/access.log
```

## View Error Logs

```bash
sudo tail -50 /var/log/nginx/error.log
```

## Save Logs to File

```bash
cat /var/log/nginx/access.log > nginx-logs.txt
```

Verify:

```bash
cat nginx-logs.txt
```

---

## Download Log File

```bash
scp -i my-key.pem ubuntu@<PUBLIC-IP>:~/nginx-logs.txt .
```

**Observation:**
Log file downloaded successfully to local machine.

---

# Commands Used

```bash
ssh -i my-key.pem ubuntu@<PUBLIC-IP>
sudo apt update
sudo apt upgrade -y
sudo apt install docker.io -y
docker --version
sudo systemctl status docker
sudo apt install nginx -y
sudo systemctl status nginx
curl http://localhost
tail -50 /var/log/nginx/access.log
tail -50 /var/log/nginx/error.log
cat /var/log/nginx/access.log > nginx-logs.txt
scp -i my-key.pem ubuntu@<PUBLIC-IP>:~/nginx-logs.txt .
```

---

# Challenges Faced

### Challenge 1

Nginx webpage was not opening initially.

### Solution

Checked AWS Security Group and opened port 80 (HTTP).

### Challenge 2

SSH connection failed.

### Solution

Verified key pair permissions and used correct public IP.

---

# What I Learned

* How to launch and access a cloud server.
* How to connect securely using SSH.
* How to install and manage Docker and Nginx.
* How AWS Security Groups control network access.
* How to view and export Nginx logs.
* How to verify a web server from the public internet.

---

# Conclusion

Successfully deployed a cloud server, installed Docker and Nginx, configured network access, verified web availability, and extracted server logs.

