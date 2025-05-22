# 🌐 Deploy Angular SSR on Ubuntu 22.04

This guide walks you through deploying an Angular Server-Side Rendered (SSR) application on an Ubuntu 22.04 VPS using **Node.js**, **PM2**, **Nginx**, and **Let's Encrypt SSL**.

---

## ✅ Prerequisites

- A VPS (Ubuntu 22.04) with root access
- A domain pointed to your VPS IP address
- Angular SSR build ready for deployment

---

## ⚙️ Step 1: Install Node.js and PM2

1. **Install Node.js (LTS version recommended):**
    ```bash
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt install -y nodejs
    ```

2. **Install PM2 globally:**
    ```bash
    sudo npm install -g pm2
    ```

3. **Verify installations:**
    ```bash
    node -v
    npm -v
    pm2 -v
    ```

---

## 📤 Step 2: Upload Angular SSR Build to VPS

Use SCP to transfer your project zip file:

```bash
scp ./yourprojectdist.zip root@<VPS_IP>:/var/www/website
```

Replace `<VPS_IP>` with your actual VPS IP address.

---

## 📦 Step 3: Unzip the Project

SSH into the server and unzip the project:

```bash
cd /var/www/website
unzip yourprojectdist.zip
```

---

## 🚀 Step 4: Run the App with PM2

Start the SSR server using PM2:

```bash
pm2 start dist/your-project-name/server/server.mjs --name your-project-name
```

> Replace `your-project-name` with your actual project name.

---

## 🔁 Step 5: Auto-start PM2 on Boot

Enable PM2 to restart your app after server reboots:

```bash
pm2 startup systemd
pm2 save
```

---

## 🌐 Step 6: Install and Configure Nginx

1. **Install Nginx:**
    ```bash
    sudo apt update
    sudo apt install nginx
    ```

2. **Create a new site config:**
    ```bash
    sudo nano /etc/nginx/sites-available/website
    ```

3. **Add the following configuration:**
    ```nginx
    server {
        listen 80;
        server_name your-domain.com www.your-domain.com;

        location / {
            proxy_pass http://localhost:4000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```

4. **Enable the site and restart Nginx:**
    ```bash
    sudo ln -s /etc/nginx/sites-available/your-domain.com /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx
    ```

---

## 🔐 Step 7: Add SSL with Let's Encrypt

1. **Install Certbot:**
    ```bash
    sudo apt install certbot python3-certbot-nginx
    ```

2. **Generate and apply SSL certificate:**
    ```bash
    sudo certbot --nginx -d your-domain.com -d www.your-domain.com
    ```

---

## ✅ Done

Your Angular SSR application is now live and secured with HTTPS! Visit:
`https://your-domain.com`

