# 🔒 How to Install an SSL Certificate on Your VPS (Using Certbot & Nginx)

Secure your website with a **free SSL certificate** from Let’s Encrypt using Certbot.

---

## ✅ Step 1: Point Your Domain to Your VPS

Update your **DNS records** in your domain registrar’s control panel:

| Type | Name | Value (Data)    | TTL     |
| ---- | ---- | --------------- | ------- |
| A    | @    | `<Your VPS IP>` | Default |

> ⚠️ Make sure to replace `yourdomain.com` with your actual domain name and use your VPS IP address.

---

## ✅ Step 2: Install Certbot and Required Packages

Update packages and install Certbot for Nginx:

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt install certbot python3-certbot-nginx -y
```

---

## ✅ Step 3: Obtain and Install the SSL Certificate

Run the following command to automatically configure HTTPS for your domain:

```bash
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

You will be prompted to:

* Enter your email address.
* Agree to the terms of service.
* Choose whether to redirect HTTP to HTTPS (recommended).

---

## 🔁 Step 4: Test Auto-Renewal (Optional but Recommended)

Let’s Encrypt certificates expire every 90 days. Certbot installs a cron job to auto-renew them. Test the renewal process with:

```bash
sudo certbot renew --dry-run
```

---

## 🎉 Done!

Your website should now be accessible via:

```
https://yourdomain.com
```

---
---