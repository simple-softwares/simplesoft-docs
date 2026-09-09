# SimpleSoft Production Server — Reference Guide

> **Security notice:** This file contains credentials. Do NOT commit to a public repo.
> Add `docs/server_stats.md` to `.gitignore` if this repo is ever pushed publicly.

---

## Server Details

| Field | Value |
|---|---|
| Provider | Hostinger (VPS) |
| Hostname | blackloop |
| IP | 150.129.147.78 |
| SSH User | digiflyer |
| OS | Ubuntu (no Docker — plain nginx + systemd) |
| SSH | `ssh digiflyer@150.129.147.78` |

---

## Domain & DNS (GoDaddy)

| Record | Name | Value |
|---|---|---|
| A | `@` | 150.129.147.78 |
| A | `*` | 150.129.147.78 |
| CNAME | `www` | simplesoft.co.in |

- `simplesoft.co.in` → Marketing website
- `*.simplesoft.co.in` → ERP app (each subdomain = one workspace)

---

## SSL Certificates (Let's Encrypt)

| Cert | Path | Covers |
|---|---|---|
| Non-wildcard | `/etc/letsencrypt/live/simplesoft.co.in/` | `simplesoft.co.in`, `www.simplesoft.co.in` |
| Wildcard | `/etc/letsencrypt/live/simplesoft.co.in-0001/` | `*.simplesoft.co.in` |

Renew wildcard cert (manual DNS-01 challenge via GoDaddy TXT record):
```bash
sudo certbot certonly --manual --preferred-challenges dns -d "*.simplesoft.co.in"
```

---

## nginx

| Config file | Serves |
|---|---|
| `/etc/nginx/sites-enabled/simplesoft` | Marketing site — `simplesoft.co.in` |
| `/etc/nginx/sites-enabled/simplesoft-app` | ERP app — `*.simplesoft.co.in` (frontend + `/api/` proxy) |
| `/etc/nginx/sites-enabled/digiflyer_migration` | `console.digiflyer.in` |

```bash
sudo nginx -t                    # test config
sudo systemctl reload nginx      # apply changes (no downtime)
sudo systemctl restart nginx     # full restart
```

---

## Backend — FastAPI

| Field | Value |
|---|---|
| Path | `/opt/simplesoft/backend/` |
| Virtualenv | `/opt/simplesoft/backend/venv/` |
| Env file | `/opt/simplesoft/backend/.env` |
| Runs on | `127.0.0.1:8000` |
| systemd service | `simplesoft.service` |

```bash
sudo systemctl status simplesoft     # check status
sudo systemctl restart simplesoft    # restart after code update
sudo journalctl -u simplesoft -f     # live logs
```

### `.env` contents
```
DATABASE_URL=postgresql+asyncpg://simplesoft:Simplesoft%402026@localhost:5432/simplesoft
SECRET_KEY=<your-secret-key>
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=10080
```

---

## Database — PostgreSQL

| Field | Value |
|---|---|
| Host | localhost:5432 |
| Database | simplesoft |
| User | simplesoft |
| Password | Simplesoft@2026 |

```bash
# Connect
sudo -u postgres psql simplesoft

# Backup (run on server)
sudo -u postgres pg_dump simplesoft > /home/digiflyer/backup_$(date +%Y%m%d).sql

# Restore from local dump (run on Windows)
scp D:/simplesoft_workspace/simplesoft_local.sql digiflyer@150.129.147.78:/tmp/
# then on server:
sudo -u postgres psql simplesoft < /tmp/simplesoft_local.sql
```

---

## Frontend — React (ERP App)

| Field | Value |
|---|---|
| Web root | `/var/www/simplesoft-app/` |
| Build command (local) | `cd frontend && npm run build` |
| Local dev workspace | set in `frontend/.env.development` as `VITE_WORKSPACE=<slug>` |

---

## Marketing Website

| Field | Value |
|---|---|
| Web root | `/var/www/simplesoft/` |
| Build command (local) | `cd simplesoft_website && npm run build` |

---

## Workspaces

### system — Super Admin

| Field | Value |
|---|---|
| URL | https://sbit.simplesoft.co.in/admin/login |
| Email | superadmin@simplesoft.com |
| Password | 12345678 |

### sbit — SBIT

| Field | Value |
|---|---|
| URL | https://sbit.simplesoft.co.in |
| Workspace slug | `sbit` |
| Admin email | admin@sbit.in |
| Password | 12345678 |
| User (Manish) | manish@networkautomation.in |
| Password | 12345678 |

### demo — Demo Workspace

| Field | Value |
|---|---|
| URL | https://demo.simplesoft.co.in |
| Workspace slug | `demo` |
| Admin email | admin@simplesoft.com |
| Password | 12345678 |

---

## Deploy Checklist — Code Update

### Backend update
```bash
# 1. Copy new backend files to server
scp -r D:/simplesoft_workspace/backend/app/ digiflyer@150.129.147.78:/opt/simplesoft/backend/app/

# 2. Install any new dependencies (if requirements.txt changed)
ssh digiflyer@150.129.147.78
cd /opt/simplesoft/backend
source venv/bin/activate
pip install -r requirements.txt

# 3. Restart service
sudo systemctl restart simplesoft
sudo journalctl -u simplesoft -f   # watch logs
```

### Frontend update
```bash
# 1. Build locally
cd D:/simplesoft_workspace/frontend
npm run build

# 2. Upload to server
scp -r D:/simplesoft_workspace/frontend/dist digiflyer@150.129.147.78:/tmp/simplesoft_new

# 3. On server: swap files
sudo cp -r /tmp/simplesoft_new/. /var/www/simplesoft-app/
sudo chown -R www-data:www-data /var/www/simplesoft-app/
```

### Marketing site update
```bash
cd D:/simplesoft_workspace/simplesoft_website
npm run build
scp -r dist digiflyer@150.129.147.78:/tmp/simplesoft_mkt
# On server:
sudo cp -r /tmp/simplesoft_mkt/. /var/www/simplesoft/
sudo chown -R www-data:www-data /var/www/simplesoft/
```

---

## Quick Health Check

```bash
# All from the server
sudo systemctl status simplesoft          # backend running?
curl -s http://127.0.0.1:8000/api/health  # API responding?
curl -sk https://sbit.simplesoft.co.in | grep '<title>'  # frontend up?
sudo -u postgres psql -c '\l' simplesoft  # DB accessible?
```


scp D:/simplesoft_workspace/frontend/dist/index.html digiflyer@150.129.147.78:/var/www/simplesoft-app/index.html && scp D:/simplesoft_workspace/frontend/dist/assets/index-Dn207Jn6.css digiflyer@150.129.147.78:/var/www/simplesoft-app/assets/index-Dn207Jn6.css && scp D:/simplesoft_workspace/frontend/dist/assets/index-BkmEXYLK.js digiflyer@150.129.147.78:/var/www/simplesoft-app/assets/index-BkmEXYLK.js && echo "deployed"