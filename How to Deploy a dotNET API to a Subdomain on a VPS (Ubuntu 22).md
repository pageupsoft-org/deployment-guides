# 🚀 How to Deploy a .NET API to a Subdomain on a VPS (Ubuntu 22)

This guide walks you through deploying a published .NET API to a subdomain using **Nginx**, **Systemd**, and **Let's Encrypt SSL** on a Linux VPS.

---

## ✅ Prerequisites

* A VPS with root access
* A domain and subdomain pointed to your VPS IP
* .NET runtime installed on the VPS
* Your .NET project is published (e.g., via `dotnet publish`)

---

## 📁 Step 1: Create a Folder and Upload the Published Files

1. **Create a folder inside `/var/www/`**

   ```bash
   cd /var/www
   mkdir api
   ```

2. **Upload your published files from your local machine**

   ```bash
   scp ./yourpublishfiles.zip root@<VPS_IP>:/var/www/api
   ```

   Replace `<VPS_IP>` with your server's IP address.

---

## 📦 Step 2: Unzip the Files on the Server

```bash
cd /var/www/api
unzip yourpublishfiles.zip
```

---

## 🧪 Step 3: Test the Application

Make sure the app runs correctly:

```bash
dotnet yourapplication.dll
```

You should see the app running on `http://localhost:5000`.

---

## ⚙️ Step 4: Create a Systemd Service

1. **Navigate to the systemd directory:**

   ```bash
   cd /etc/systemd/system
   ```

2. **Create the service file:**

   ```bash
   sudo nano yourapplication-api.service
   ```

3. **Paste the following configuration:**

   ```ini
   [Unit]
   Description=YourProject API

   [Service]
   WorkingDirectory=/var/www/api
   ExecStart=/usr/bin/dotnet /var/www/api/yourapplication.dll
   Restart=always
   RestartSec=10
   KillSignal=SIGINT
   SyslogIdentifier=yourapplication-api
   User=root
   Environment=ASPNETCORE_ENVIRONMENT=Production
   Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

   [Install]
   WantedBy=multi-user.target
   ```

4. **Save and exit:**

   * Press `Ctrl + O` then `Enter` to save
   * Press `Ctrl + X` to exit

---

## 🌐 Step 5: Install and Configure Nginx for the Subdomain

1. **Install Nginx:**

   ```bash
   sudo apt-get update
   sudo apt-get install nginx
   ```

2. **Create Nginx config file for the API:**

   ```bash
   cd /etc/nginx/sites-available
   sudo nano api
   ```

3. **Add the following configuration:**

   ```nginx
   server {
       listen 80;
       server_name api.yourdomain.com;  # Replace with your actual subdomain

       location / {
           proxy_pass http://localhost:5000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection keep-alive;
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

4. **Enable the site by creating a symlink:**

   ```bash
   sudo ln -s /etc/nginx/sites-available/api /etc/nginx/sites-enabled/
   ```

5. **Test the Nginx configuration:**

   ```bash
   sudo nginx -t
   ```

6. **Reload Nginx if the test is successful:**

   ```bash
   sudo systemctl reload nginx
   ```

---

## 🔁 Step 6: Start and Enable the .NET Service

```bash
sudo systemctl start yourapplication-api.service
sudo systemctl enable yourapplication-api.service
```

---

## 📋 Step 7: Check Service Status

```bash
sudo systemctl status yourapplication-api.service
```

Make sure it says `active (running)`.

---

## 🔒 Step 8: Secure with SSL (Let's Encrypt)

1. **Install Certbot:**

   ```bash
   sudo apt-get install certbot python3-certbot-nginx
   ```

2. **Generate and apply the SSL certificate:**

   ```bash
   sudo certbot --nginx -d api.yourdomain.com
   ```

---

## 🧪 Step 9: Test Your Application

Open your browser and navigate to:

```
https://api.yourdomain.com
```

You should see your .NET API running securely over HTTPS.

---
---