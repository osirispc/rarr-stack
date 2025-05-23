# rarr-stack

Self-hosted *arr media automation stack with VPN protection and reverse proxy access. Built for Proxmox VM environments using Docker, Samba-mounted ZFS shares, and NordVPN routing.

---

## 🧩 Stack Overview

| Component      | Purpose                                   |
|----------------|-------------------------------------------|
| **Radarr**     | Automate movie downloads                  |
| **Sonarr**     | Automate TV show downloads                |
| **Lidarr**     | Automate music library                    |
| **Readarr**    | Automate eBook and audiobook downloads    |
| **Prowlarr**   | Indexer aggregator for the entire stack   |
| **Transmission** + VPN | Torrent client routed via NordVPN |
| **Nginx Proxy Manager** | Secure HTTPS access via DuckDNS  |

All media traffic is routed through a VPN network. Only the proxy is publicly exposed.

---

## 🗃️ Folder Structure

```text
rarr-stack/
├── docker/
│   ├── transmission-vpn.yml      # VPN tunnel
│   └── rarr-stack.yml            # *arr containers
├── config/
│   ├── portainer-setup.md
│   └── smb-mount-setup.md
├── reverse-proxy/
│   └── nginx-proxy-manager.md
├── media-paths.md                # ZFS and mount mappings
├── .env.example                  # Environment variables
