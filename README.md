<p align="right">
  🌐 <a href="README-en.md">English</a> | <strong>Deutsch</strong>
</p>

# 🛡️ NetBird + Authentik + Caddy / Traefik #

Self-Hosted Zero-Trust Networking – Alles auf einem Host
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
# 📘 Vollständige Anleitung #

Die komplette Dokumentation findest du hier:

👉 [Zum vollständigen Artikel](https://jusec.me/netbird-authentik)￼

---
# 🚀 Überblick #

Dieses Repository automatisiert das Setup von:

- 🔐 NetBird – Zero-Trust VPN
- 👤 Authentik – Identity Provider (OIDC)
- 🔁 Caddy **oder** Traefik – Reverse-Proxy mit automatischem TLS
- 🖥️ Portainer – Kompatible Stack-Templates für einfaches Deployment
- ⚙️ Automatische Setup-Skripte
- 🔒 Optionale Firewall-Härtung
- 🧩 Ein Host reicht völlig aus

---
# 📂 Repository-Struktur #
```
/
├── caddy/                        # Caddy Reverse Proxy
│   ├── docker-compose.yml
│   └── generate-authentik-caddyfile.sh
├── traefik/                      # Traefik Reverse Proxy
│   ├── docker-compose.yml
│   └── generate-authentik-traefik.sh
├── authentik/                    # Identity Provider
│   ├── docker-compose.yml            # Caddy-Variante
│   ├── docker-compose.traefik.yml    # Traefik-Variante
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
│   └── docker-compose.*.yml.tmpl     # Diverse Deployment-Templates
├── portainer-templates.json      # Portainer App-Templates
├── firewall.sh
├── README.md
└── README-en.md
```
---
# 🧠 Schnellstart #

## Option A: Mit Caddy (Standard)

1. Repository klonen
```
git clone https://github.com/jusecdev/netbird-authentik-setup.git
cd netbird-authentik-setup
```
2. Caddy installieren & konfigurieren
3. Authentik einrichten
```
sudo bash authentik/authentik-init.sh
```
4. NetBird konfigurieren & starten
5. Optional Firewall aktivieren
```
sudo bash firewall.sh
```

## Option B: Mit Traefik

1. Repository klonen
```
git clone https://github.com/jusecdev/netbird-authentik-setup.git
cd netbird-authentik-setup
```
2. Traefik starten
```
cd traefik
docker compose up -d
```
3. Authentik einrichten (Traefik-Variante)
```
cd ../authentik
bash authentik-init.sh
docker compose -f docker-compose.traefik.yml up -d
bash ../traefik/generate-authentik-traefik.sh
```
4. NetBird konfigurieren (`setup.env` anlegen, Modus auf `proxy_docker_traefik` setzen)
```
cd ../netbird
cp setup.env.authentik.example setup.env
# In setup.env: NETBIRD_DEPLOYMENT_MODE="proxy_docker_traefik" setzen
bash configure.sh
docker compose -f artifacts/docker-compose.yml up -d
```
5. Optional Firewall aktivieren
```
sudo bash ../firewall.sh
```

## Option C: Mit Portainer

Alle docker-compose-Dateien lassen sich direkt als Portainer-Stacks deployen.
Die Datei `portainer-templates.json` kann als App-Template-Quelle in Portainer
eingebunden werden:

1. In Portainer: **Settings** → **App Templates** → URL setzen auf:
```
https://raw.githubusercontent.com/jusecdev/netbird-authentik-setup/main/portainer-templates.json
```
2. Templates nutzen, um Traefik, Authentik und NetBird als Stacks zu deployen.

---
# 🔄 Deployment-Modi #

| Modus | Beschreibung |
|---|---|
| `standalone` | NetBird terminiert TLS selbst (Let's Encrypt) |
| `proxy_docker` | Externer Reverse-Proxy im Docker-Netzwerk (Caddy oder Traefik) |
| `proxy_external` | Reverse-Proxy auf einem anderen Host |
| `proxy_docker_caddy` | Caddy im selben Compose integriert |
| `proxy_docker_traefik` | Traefik im selben Compose integriert (Docker-Labels) |

---
# 🛠️ Voraussetzungen #
- Öffentlich erreichbarer Server
- Domain + DNS Einträge
- Docker & Docker Compose

---
# 🔒 Firewall-Härtung #
```
sudo bash firewall.sh
```
---
# ⭐ Unterstützen #

Wenn dir dieses Projekt geholfen hat:
⭐ Gib dem Repo einen Stern!

---
# 📜 Lizenz #

MIT-Lizenz
