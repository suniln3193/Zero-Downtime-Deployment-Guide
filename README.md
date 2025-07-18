
# Zero-Downtime Deployment Guide

This project uses a simple and reliable `.next` folder swap method to ensure **zero-downtime deployments** on a self-hosted VPS using **PM2** and **NGINX**.

---

## 📂 Folder Structure

| Folder Path                          | Purpose                   |
|--------------------------------------|---------------------------|
| `/var/www/anand-rathi-web/`          | Live production website   |
| `/var/www/anand-rathi-web-backup/`   | Build & testing workspace |

---

## 🚀 Deployment Steps

### 1️⃣ Build in Backup Folder

```bash
cd /var/www/anand-rathi-web-backup
git pull origin main

npm install
npm run build  # Generates fresh .next folder here
```

Optional: Test the build locally before deployment.

```bash
PORT=4000 npm start
# Visit http://your-server-ip:4000 to verify
```

---

### 2️⃣ Swap .next Folder into Live

Backup the current live build (optional):

```bash
cd /var/www/anand-rathi-web
mv .next .next-backup-$(date +%Y%m%d%H%M%S)
```

Copy new build from backup:

```bash
cp -R /var/www/anand-rathi-web-backup/.next /var/www/anand-rathi-web/
```

---

### 3️⃣ Restart Next.js Server (PM2 Example)

```bash
pm2 reload anand-rathi-web
```

If not using PM2:

```bash
pkill -f 'next start'
cd /var/www/anand-rathi-web
PORT=3000 npm start &
```

---

## ✅ Rollback (If Needed)

If something goes wrong:

```bash
rm -rf /var/www/anand-rathi-web/.next
mv /var/www/anand-rathi-web/.next-backup-YYYYMMDDHHMMSS /var/www/anand-rathi-web/.next
pm2 reload anand-rathi-web
```

---

## ⚡ Example Deployment Script (`deploy.sh`)

```bash
#!/bin/bash

LIVE_FOLDER="/var/www/anand-rathi-web"
BACKUP_FOLDER="/var/www/anand-rathi-web-backup"

# Backup current build
cd "$LIVE_FOLDER"
mv .next .next-backup-$(date +%Y%m%d%H%M%S)

# Copy new build
cp -R "$BACKUP_FOLDER/.next" "$LIVE_FOLDER/"

# Reload PM2
pm2 reload anand-rathi-web

echo "Deployment complete."
```

**Usage:**

1. Build in the backup folder as shown earlier.
2. Run the script after build completes:

```bash
bash deploy.sh
```

---

## 📌 Notes

- Always test the build locally before swapping to avoid downtime.
- Keep periodic backups of `.next` folders for safety.
- Consider automating full CI/CD for further optimization.
