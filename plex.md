# 🏠 Raspberry Pi Home Media Server Setup Guide

This guide explains how to turn your Raspberry Pi into a powerful home media server using **Plex** and **qBittorrent**, accessible through a web interface and shareable via the network using **Samba**.

---

## 📡 Accessing Raspberry Pi via SSH (Windows)

1. **Install PuTTY**  
   Download and install [PuTTY](https://www.putty.org/) on your Windows PC.

2. **Connect via SSH**
   - Open PuTTY.
   - Enter your Raspberry Pi's IP address in the **Host Name** field.
   - Set **Port** to `22` and **Connection type** to `SSH`.
   - Click **Open** and log in with your Raspberry Pi credentials.

---

## 🎞️ Install and Configure Plex Server

1. **Update the system**
   ```bash
   sudo apt update
   sudo apt upgrade
