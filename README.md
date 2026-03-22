# 🦊 GitLab Deployment with Docker Compose & Traefik

This repository provides a ready-to-use setup for deploying **GitLab CE** locally using Docker Compose, exposed securely through **Traefik** with HTTPS.

It is designed for development, testing, or homelab environments using a local domain (`*.test.local`).

---

## ✨ Features

- GitLab CE deployed via Docker
- HTTPS exposure using Traefik (`gitlab.test.local`)
- Reverse proxy support (Traefik → GitLab internal NGINX)
- Persistent data (config, logs, repositories)
- SSH access for Git over port `2222`
- Compatible with self-signed certificates

---

## 📁 Project Structure

```
.  
├── docker-compose.yml  
├── config/        # GitLab configuration  
├── logs/          # GitLab logs  
├── data/          # Git repositories and application data
```

---

## How It Works

### 👉 Architecture Overview

- Traefik handles incoming HTTPS traffic
- Requests are forwarded to GitLab’s internal NGINX (HTTP only)
- GitLab is configured to trust the reverse proxy
- SSL termination is handled by Traefik

---

## 🔧 Requirements

- Docker
- Docker Compose
- Traefik already deployed and connected to `traefik_network`
- Local DNS or `/etc/hosts` configured

Example:

```
127.0.0.1 gitlab.test.local
```

---

## ▶️ Start GitLab

```bash
docker compose up -d
```


⚠️ First startup can take several minutes (GitLab initialization is slow).

---

## Access GitLab

Open your browser:

```
https://gitlab.test.local
```

⚠️ If using a self-signed certificate, your browser will show a warning.

---

## Initial Root Password

After the first startup, retrieve the root password:

```bash
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

---

## SSH Access

Git over SSH is available via port **2222**:

```bash
git clone ssh://git@gitlab.test.local:2222/username/repo.git
```

---

## Persistent Data

All GitLab data is stored locally:

- `./config` → GitLab configuration
- `./logs` → logs
- `./data` → repositories and database

---

## 🧠 Important Notes

- GitLab runs behind Traefik → SSL is terminated at Traefik
- Internal GitLab NGINX runs only on HTTP
- `trusted_proxies` must match your Docker network range
- Ensure enough resources:
    - Minimum **4GB RAM recommended**
    - GitLab is resource-heavy

---

## 📜 License

This project is intended for personal use, testing, and development environments.
