# 🌐 Deploy Next.js SSR on Ubuntu

This guide walks you through deploying a Next.js Server-Side Rendered (SSR) application on an Ubuntu 22.04 VPS using **Node.js**, **PM2**, **Nginx**, and **Let's Encrypt SSL**.

---

## ✅ Prerequisites

- A VPS (Ubuntu 22.04) with root access
- A domain pointed to your VPS IP address (e.g., `82.29.166.128`)
- Next.js SSR build ready for deployment (generated via `npm run build`)

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

## 📤 Step 2: Upload Next.js Build to VPS

Use SCP to transfer your project files (`.next`, `package.json`, `package-lock.json`, and optionally `.env`):

```bash
scp -r ./package.json ./package-lock.json ./.next root@<VPS_IP>:/var/www/html
```

Replace `<VPS_IP>` with your actual VPS IP address (e.g., `82.29.166.128`).

---

## 📦 Step 3: Install Dependencies

SSH into the server and install the project dependencies:

```bash
cd /var/www/html
npm ci --production
```

> **Note**: Using `npm ci --production` installs only production dependencies, optimizing for deployment.

---

## 🚀 Step 4: Run the App with PM2

Start the Next.js server using PM2:

```bash
pm2 start npm --name "seo-ssr" -- run start
```

> **Note**: This assumes your `package.json` has a `start` script defined as `"start": "next start"`. Replace `seo-ssr` with your desired app name.

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
    sudo nano /etc/nginx/sites-available/your-domain.com
    ```

3. **Add the following configuration:**
    ```nginx
    server {
        listen 80;
        server_name your-domain.com www.your-domain.com;

        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```

    > **Note**: Next.js runs on port 3000 by default. Adjust `proxy_pass` if you use a different port (e.g., `next start -p 4000`).

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

Your Next.js SSR application is now live and secured with HTTPS! Visit:
`https://your-domain.com`