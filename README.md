# Anand Rathi Web – Zero-Downtime Deployment Guide

This project uses a simple and reliable `.next` folder swap method to ensure **zero-downtime deployments** on a self-hosted VPS using **PM2** and **NGINX**.

---

## 📂 Folder Structure

| Folder Path                          | Purpose                    |
|--------------------------------------|----------------------------|
| `/var/www/anand-rathi-web/`          | Live production website    |
| `/var/www/anand-rathi-web-backup/`   | Build & testing workspace  |

---

## 🚀 Deployment Steps

### 1️⃣ Build in Backup Folder

```bash
cd /var/www/anand-rathi-web-backup
git pull origin main

npm install
npm run build
