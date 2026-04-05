# 🚀 Nginx Installation & Setup Guide (Ubuntu 24.04 LTS)

This guide provides a **structured, production-ready approach** to install, configure, and run Nginx on Ubuntu.

Nginx is widely used as:
- Reverse proxy
- Load balancer
- Static file server
- Web server for high-performance applications

---

## 📌 Table of Contents

- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Firewall Configuration](#-firewall-configuration)
- [Service Management](#-service-management)
- [Verify Installation](#-verify-installation)
- [Directory Structure](#-directory-structure)
- [Server Block Setup](#-server-block-setup)
- [Basic Reverse Proxy Example](#-basic-reverse-proxy-example)
- [Troubleshooting](#-troubleshooting)

---

## 🧱 Prerequisites

- Ubuntu 24.04 LTS
- Sudo privileges
- Internet connection

---

## ⚙️ Installation

### Step 1: Update system

```bash
sudo apt update
```
### Step 2: Install Nginx
```bash
sudo apt install -y nginx
```
🔥 Firewall Configuration

List available profiles:
```bash
sudo ufw app list
```
Allow HTTP & HTTPS traffic:
```bash
sudo ufw allow 'Nginx Full'
```
🔄 Service Management
Check status:
```bash
sudo systemctl status nginx
```
Start Sevice:
```bash
sudo systemctl start nginx
```
Enable auto-start:
```bash
sudo systemctl enable nginx
```
Restart:
```bash
sudo systemctl restart nginx
```
Reload (zero downtime):
```bash
sudo systemctl reload nignx
```

✅ Verify Installation

Open browser:
```bash
http://YOUR_SERVER_IP
```
👉 You should see:
“Welcome to nginx!”


```bash 📁 Directory Structure
Path	Purpose
/var/www/html	Default web root
/etc/nginx	Configuration directory
/etc/nginx/nginx.conf	Main config file
/etc/nginx/sites-available/	Site configs
/etc/nginx/sites-enabled/	Active sites
```
### 🌐 Server Block Setup (Virtual Host)
## Step 1: Create project directory
```bash
sudo mkdir -p /var/www/your_domain/html
```
## Step 2: Set permissions
```bash
sudo chown -R $USER:$USER /var/www/your_domain/html
```
## Step 3: Create sample page
```bash
nano /var/www/your_domain/html/index.html
```
```bash
<h1>Welcome to your_domain</h1>
```
## Step 4: Create server block config
```bash
sudo nano /etc/nginx/sites-available/your_domain
```
```bash
server {
    listen 80;
    server_name your_domain www.your_domain;

    root /var/www/your_domain/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
## Step 5: Enable site
```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```
## Step 6: Test config
```bash
sudo nginx -t
```
## Step 7: Restart Nginx
```bash
sudo systemctl restart nginx
```
## 🔁 Reverse Proxy Example (Django / Node.js)
```bash
sudo nano /etc/nginx/sites-available/app
server {
    listen 80;
    server_name your_domain;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
Enable:
```bash
sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```
🛠️ Troubleshooting
Check status
```bash
sudo systemctl status nginx
```
Check config errors
```bash
sudo nginx -t
```
View logs
```bash
sudo tail -f /var/log/nginx/error.log
```
Check port usage
```bash
sudo ss -plnt | grep :80
```
⚠️ Common Pitfalls
- Nginx not restarted after config change
- Port 80 blocked by firewall
- Syntax error in config
- Incorrect root path
- Duplicate server_name conflicts