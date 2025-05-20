# 🌐 How to Host an HTML File on a VPS Using Nginx

This guide will help you quickly host a static HTML file using **Nginx** on a Linux-based VPS.

---

## ✅ Prerequisites

* A VPS with root or sudo access
* A domain or IP address to access your server
* Your HTML file(s) ready for deployment (e.g., `index.html`)

---

## 📥 Step 1: Install Nginx

Update your package list and install Nginx:

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install nginx
```

After installation, Nginx should start automatically. You can check its status with:

```bash
sudo systemctl status nginx
```

---

## 📁 Step 2: Upload Your HTML File

1. **Navigate to Nginx's default root directory:**

   ```bash
   cd /var/www/html
   ```

2. **Remove the default index page (optional):**

   ```bash
   sudo rm index.nginx-debian.html
   ```

3. **Upload your `index.html` (from your local machine):**

   On your local machine, use `scp` or an SFTP client like FileZilla. For example:

   ```bash
   scp ./index.html root@<VPS_IP>:/var/www/html/
   ```

   Replace `<VPS_IP>` with your server's IP address.

---

## 🌍 Step 3: Access Your Website

Open your browser and go to:

```
http://<VPS_IP>
```

Or, if you’ve set up a domain:

```
http://yourdomain.com
```

You should see your `index.html` file rendered in the browser.

---

## 🔐 Optional: Secure Your Site with HTTPS (Let's Encrypt)

1. **Install Certbot and the Nginx plugin:**

   ```bash
   sudo apt-get install certbot python3-certbot-nginx
   ```

2. **Run Certbot to obtain and install your SSL certificate:**

   ```bash
   sudo certbot --nginx -d yourdomain.com
   ```

3. **Verify SSL:**

   Visit:

   ```
   https://yourdomain.com
   ```

---
---