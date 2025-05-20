# 🛠️ How to Install SQL Server on Ubuntu 22 VPS

This step-by-step guide helps you install **Microsoft SQL Server 2022** on a VPS running **Ubuntu 22.04**, including optional CLI tools like `sqlcmd`.

---

## ✅ Step 1: Import the Microsoft GPG Key

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
```

---

## ✅ Step 2: Add the SQL Server Repository

```bash
sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/22.04/mssql-server-2022.list)"
```

---

## ✅ Step 3: Install SQL Server

```bash
sudo apt update
sudo apt install -y mssql-server
```

---

## ✅ Step 4: Run the SQL Server Setup

```bash
sudo /opt/mssql/bin/mssql-conf setup
```

During setup, you'll be prompted to:

* Select an **edition** (e.g., `Developer` for free use).
* Accept the **license terms**.
* Set a **SA (System Administrator)** password.

---

## 🔁 Step 5: Enable and Start the SQL Server Service

```bash
sudo systemctl enable --now mssql-server
```

Check the status to ensure it’s running:

```bash
systemctl status mssql-server
```

---

## 🧰 (Optional) Install SQL Server Command-Line Tools

### 1. Add the Tools Repository

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list)"
```

### 2. Install SQL Tools

```bash
sudo apt update
sudo apt install -y mssql-tools unixodbc-dev
```

### 3. Add `sqlcmd` to PATH

```bash
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```

---

## 🔍 Step 6: Test the Connection

Use the following command to connect to SQL Server:

```bash
sqlcmd -S localhost -U SA -P '<YourPassword>'
```

Replace `<YourPassword>` with the password you set during the setup.

---

## 🔐 Firewall Tip

Ensure **port `1433`** (default for SQL Server) is open in your firewall to allow remote connections:

```bash
sudo ufw allow 1433/tcp
```

---
---