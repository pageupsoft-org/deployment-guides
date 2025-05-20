# ⚙️ How to Install .NET 8 SDK and Runtime on Ubuntu 22

This guide walks you through installing the **.NET 8 SDK and ASP.NET Core Runtime** on Ubuntu 22.04 using Microsoft’s official package repository.

---

## ✅ Step 1: Add Microsoft Package Repository

1. **Download and register the Microsoft package signing key:**

```bash
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

2. **Install required dependencies:**

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates
```

---

## ✅ Step 2: Install .NET 8

### Option A: Install the .NET SDK (for development and building apps)

```bash
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0
```

### Option B: Install the .NET Runtime (for hosting only)

```bash
sudo apt-get install -y aspnetcore-runtime-8.0
```

> ⚠️ **Note:** If you're deploying an app to the server and won’t be building code there, you only need the **runtime**.

---

## ✅ Step 3: Verify the Installation

Check your installed .NET version:

```bash
dotnet --version
```

You should see output like:

```
8.0.x
```

---
---