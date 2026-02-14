<p align="right">
  🌐 <strong>English</strong> | <a href="README.md">Deutsch</a>
</p>

# 🛡️ NetBird + Authentik + Caddy / Traefik #

Self-Hosted Zero-Trust Networking – All on a Single Host
<p align="center">
  <a href="https://github.com/jusecdev/netbird-authentik-setup/stargazers">
    <img src="https://img.shields.io/github/stars/jusecdev/netbird-authentik-setup?style=flat-square" />
  </a>
  <a href="https://github.com/jusecdev/netbird-authentik-setup/issues">
    <img src="https://img.shields.io/github/issues/jusecdev/netbird-authentik-setup?style=flat-square" />
  </a>
  <img src="https://img.shields.io/badge/Docker-Ready-blue?style=flat-square&logo=docker" />
  <img src="https://img.shields.io/badge/Authentik-OIDC-green?style=flat-square" />
  <img src="https://img.shields.io/badge/Caddy-Automatic%20TLS-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/Traefik-Reverse%20Proxy-blue?style=flat-square&logo=traefikproxy" />
  <img src="https://img.shields.io/badge/Portainer-Compatible-blue?style=flat-square&logo=portainer" />
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square" />
</p>

---
# 📘 Full Guide #
The full documentation is available here:

👉 [Full article (German)](https://jusec.me/netbird-authentik)￼

---
# 🚀 Overview #

This repository automates the setup of:
- 🔐 NetBird – Zero-Trust VPN
- 👤 Authentik – Identity Provider (OIDC)
- 🔁 Caddy **or** Traefik – Reverse Proxy with automatic TLS
- 🖥️ Portainer – Compatible stack templates for easy deployment
- ⚙️ Automated setup scripts
- 🔒 Optional firewall hardening
- 🧩 Everything runs on a single host

---
# 📂 Repository Structure #
```
/
├── caddy/                        # Caddy Reverse Proxy
│   ├── docker-compose.yml
│   └── generate-authentik-caddyfile.sh
├── traefik/                      # Traefik Reverse Proxy
│   ├── docker-compose.yml
│   └── generate-authentik-traefik.sh
├── authentik/                    # Identity Provider
│   ├── docker-compose.yml            # Caddy variant
│   ├── docker-compose.traefik.yml    # Traefik variant
│   └── authentik-init.sh
├── netbird/                      # NetBird VPN
│   ├── configure.sh
│   ├── setup.env.example
│   ├── setup.env.authentik.example
│   ├── base.setup.env
│   ├── management.json.tmpl
│   ├── turnserver.conf.tmpl
│   ├── Caddyfile.tmpl
│   ├── traefik-dynamic.yml.tmpl
│   └── docker-compose.*.yml.tmpl     # Various deployment templates
├── portainer-templates.json      # Portainer App Templates
├── firewall.sh
├── README.md
└── README-en.md
```

---
# 🧠 Quick Start

## Option A: With Caddy (Default)

1. Clone repository
```
git clone https://github.com/jusecdev/netbird-authentik-setup.git
cd netbird-authentik-setup
```
2. Install & configure Caddy
3. Set up Authentik
```
sudo bash authentik/authentik-init.sh
```
4. Configure & start NetBird
5. Optionally enable firewall
```
sudo bash firewall.sh
```

## Option B: With Traefik

1. Clone repository
```
git clone https://github.com/jusecdev/netbird-authentik-setup.git
cd netbird-authentik-setup
```
2. Start Traefik
```
cd traefik
docker compose up -d
```
3. Set up Authentik (Traefik variant)
```
cd ../authentik
bash authentik-init.sh
docker compose -f docker-compose.traefik.yml up -d
bash ../traefik/generate-authentik-traefik.sh
```
4. Configure NetBird (create `setup.env`, set mode to `proxy_docker_traefik`)
```
cd ../netbird
cp setup.env.authentik.example setup.env
# In setup.env: set NETBIRD_DEPLOYMENT_MODE="proxy_docker_traefik"
bash configure.sh
docker compose -f artifacts/docker-compose.yml up -d
```
5. Optionally enable firewall
```
sudo bash ../firewall.sh
```

## Option C: With Portainer

All docker-compose files can be deployed directly as Portainer stacks.
The file `portainer-templates.json` can be used as an App Templates source
in Portainer:

1. In Portainer: **Settings** → **App Templates** → Set URL to:
```
https://raw.githubusercontent.com/jusecdev/netbird-authentik-setup/main/portainer-templates.json
```
2. Use the templates to deploy Traefik, Authentik, and NetBird as stacks.

---
# 🔄 Deployment Modes #

| Mode | Description |
|---|---|
| `standalone` | NetBird terminates TLS itself (Let's Encrypt) |
| `proxy_docker` | External reverse proxy in Docker network (Caddy or Traefik) |
| `proxy_external` | Reverse proxy on a different host |
| `proxy_docker_caddy` | Caddy integrated in the same Compose file |
| `proxy_docker_traefik` | Traefik integrated in the same Compose file (Docker labels) |

---
# 🛠️ Requirements #
- Publicly reachable server
- Domain + DNS records
- Docker & Docker Compose

---
# 🔒 Firewall Hardening #
```
sudo bash firewall.sh
```
---
# ⭐ Support

If this project helped you:
⭐ Give the repo a star!

---
# 📜 License

MIT License