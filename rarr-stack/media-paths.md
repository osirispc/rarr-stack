📋 Full media-paths.md

# 📁 Media Path Mapping – rarr-stack

This file documents how media content is mounted and accessed throughout the stack: from Proxmox → VM → Docker → Container.

---

## 🧱 Host: Proxmox

ZFS dataset layout:

mypool/shares/ ├── downloads/ ├── movies/ ├── shows/ ├── music/ └── books/

Exported via Samba:
- From `theblacklodge`: `//192.168.199.101/shares`
- Or from `thewhitelodge`: `//192.168.199.100/shares`

---

## 🖥️ VM: rarr (Ubuntu Server)

Samba share mounted at boot to:

/mnt/data/ ├── downloads ├── movies ├── shows ├── music └── books

Mounted via `/etc/fstab`:
```fstab
//192.168.199.101/shares /mnt/data cifs credentials=/etc/smbcredentials/blacklodge.cred,iocharset=utf8,vers=3.0,noperm,noserverino,file_mode=0777,dir_mode=0777 0 0


---

🐳 Docker: Container Bind Mounts

Host Path	Container Path	Used By

/mnt/data/movies	/movies	Radarr
/mnt/data/shows 	/tv	Sonarr
/mnt/data/music         /music	Lidarr
/mnt/data/books	        /books	Readarr
/mnt/data/downloads	/downloads	All (*arr + qBittorrent)
/mnt/data/*arr	        /config	Each container's settings


Each container accesses config and media content via /mnt/data.


---

🔐 Permissions

PUID and PGID must match your VM user (e.g., id osirisortiz)

Ensure Samba mounts are writable by Docker containers

Make sure /mnt/data is mounted before Docker starts



---

🔁 Path Flow Summary

[ZFS on Proxmox] → [Samba Share] → [rarr VM /mnt/data] → [Docker Container Mount]

Examples:

mypool/shares/movies     → /mnt/data/movies     → /movies
mypool/shares/shows      → /mnt/data/shows      → /shows
mypool/shares/downloads  → /mnt/data/downloads  → /downloads
mypool/shares/music      → /mnt/data/music      → /music
mypool/shares/books      → /mnt/data/books      → /books


---

Everything flows from ZFS through to containers, cleanly and consistently.

---

Paste that into `media-paths.md`, save, commit, and push.

Then we’ll move to `nginx-proxy-manager.md` unless you say otherwise.

