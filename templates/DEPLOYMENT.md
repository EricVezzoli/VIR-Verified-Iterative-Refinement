# Production Deployment

> All commands run as your login user unless otherwise noted.
> Project is deployed to `/opt/your-project`, running as a dedicated system user.

---

## 1. Fresh Server Setup

### 1a. System packages

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3 python3-venv python3-dev \
  postgresql postgresql-contrib git
```

### 1b. Create a dedicated system user

```bash
sudo useradd -r -m -s /usr/sbin/nologin appuser
```

### 1c. Clone repository

```bash
sudo mkdir -p /opt/your-project
sudo chown appuser:appuser /opt/your-project
sudo -u appuser git clone <REPO_URL> /opt/your-project
```

### 1d. Python virtual environment

```bash
sudo -u appuser python3 -m venv /opt/your-project/.venv
sudo -u appuser /opt/your-project/.venv/bin/pip install -r requirements.txt
```

Verify:

```bash
sudo -u appuser /opt/your-project/.venv/bin/python3 -c \
  "import your_package; print('All dependencies OK')"
```

---

## 2. Database Setup

### 2a. Create database and role

```bash
sudo -u postgres psql -c "CREATE ROLE appuser WITH LOGIN PASSWORD '<STRONG_PASSWORD>';"
sudo -u postgres psql -c "CREATE DATABASE your_db OWNER appuser;"
```

### 2b. Apply schema

```bash
sudo -u appuser psql -d your_db -f /opt/your-project/schema.sql
```

---

## 3. Environment Configuration

```bash
sudo -u appuser cp /opt/your-project/.env_example /opt/your-project/.env
sudo -u appuser nano /opt/your-project/.env
# Fill in all required values (see docs/PRD.md §Config)
```

---

## 4. Systemd Services

### 4a. Main application service

```ini
# /etc/systemd/system/your-project.service
[Unit]
Description=Your Project Main Service
After=postgresql.service
Requires=postgresql.service

[Service]
Type=simple
User=appuser
WorkingDirectory=/opt/your-project
ExecStart=/opt/your-project/.venv/bin/python3 -m your_package.main
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

### 4b. Enable and start

```bash
sudo systemctl daemon-reload
sudo systemctl enable your-project
sudo systemctl start your-project
```

---

## 5. Deploy Script

```bash
#!/bin/bash
# scripts/deploy.sh — automated deploy
set -euo pipefail

cd /opt/your-project
sudo -u appuser git fetch origin main
sudo -u appuser git reset --hard origin/main
sudo -u appuser /opt/your-project/.venv/bin/pip install -r requirements.txt
# [Optional: schema migration]
sudo systemctl restart your-project
echo "Deploy complete"
```

---

## 6. Monitoring

### Health check

```bash
# Check service status
sudo systemctl status your-project

# View recent logs
sudo journalctl -u your-project -n 50 --no-pager
```

### Log rotation

Journald handles rotation by default. For file-based logs, add a logrotate config.

---

## 7. Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Service won't start | Missing `.env` | Check all required env vars are set |
| Database connection refused | PostgreSQL not running | `sudo systemctl start postgresql` |
| Permission denied | Wrong file ownership | `sudo chown -R appuser:appuser /opt/your-project` |

---

## 8. Rollback

```bash
# Revert to previous commit
cd /opt/your-project
sudo -u appuser git log --oneline -5  # find the commit to revert to
sudo -u appuser git reset --hard <commit-hash>
sudo systemctl restart your-project
```
