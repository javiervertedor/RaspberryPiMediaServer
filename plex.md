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
   ```

2. **Install Plex Media Server**
   ```bash
   curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
   echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
   sudo apt update
   sudo apt install plexmediaserver
   ```

3. **Enable and start the Plex service**
   ```bash
   sudo systemctl start plexmediaserver
   sudo systemctl enable plexmediaserver
   ```

4. **Access Plex Interface**  
   Open a browser and go to:  
   `http://<Raspberry_Pi_IP>:32400/web`

---

## 🌐 Install and Configure qBittorrent (Headless)

1. **Install qBittorrent-nox**
   ```bash
   sudo apt install qbittorrent-nox
   ```

2. **Create and set up qbittorrent user**
   ```bash
   sudo adduser --system --group qbittorrent
   sudo mkdir /home/qbittorrent
   sudo chown qbittorrent:qbittorrent /home/qbittorrent
   sudo usermod -d /home/qbittorrent qbittorrent
   ```

3. **Run qBittorrent**
   ```bash
   sudo -u qbittorrent qbittorrent-nox
   ```

4. **Access qBittorrent Web UI**  
   Open a browser:  
   `http://<Raspberry_Pi_IP>:8080`  
   Default login:  
   - Username: `admin`  
   - Password: `adminadmin`

---

## 🔁 Auto-Start qBittorrent with systemd

1. **Create a systemd service**
   ```bash
   sudo nano /etc/systemd/system/qbittorrent.service
   ```

2. **Paste the following:**
   ```ini
   [Unit]
   Description=qBittorrent Daemon Service
   After=network.target

   [Service]
   User=qbittorrent
   ExecStart=/usr/bin/qbittorrent-nox
   Restart=on-failure

   [Install]
   WantedBy=multi-user.target
   ```

3. **Enable and start service**
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable qbittorrent
   sudo systemctl start qbittorrent
   ```

---

## 🎬 Download Torrents to Plex Folder

1. **Set download folder in qBittorrent**
   - Open qBittorrent Web UI.
   - Go to `Tools > Options > Downloads`.
   - Set "Default Save Path" to:
     ```
     /var/lib/plexmediaserver/Library/Movies
     ```

2. **Set permissions for Plex access**
   ```bash
   sudo mkdir -p /var/lib/plexmediaserver/Library/Movies
   sudo chown -R plex:plex /var/lib/plexmediaserver/Library/Movies
   sudo chmod -R 755 /var/lib/plexmediaserver/Library/Movies
   ```

---

## 📁 Enable Network Sharing with Samba

1. **Install Samba**
   ```bash
   sudo apt update
   sudo apt install samba
   ```

2. **Create shared group and add users**
   ```bash
   sudo groupadd smbgroup
   sudo usermod -aG smbgroup francisco
   sudo usermod -aG smbgroup plex
   sudo usermod -aG smbgroup qbittorrent
   ```

3. **Configure Samba**
   ```bash
   sudo nano /etc/samba/smb.conf
   ```

   Add at the end of the file:
   ```ini
   [Media_Library]
   path = /var/lib/plexmediaserver/Library
   available = yes
   valid users = @smbgroup
   read only = no
   browsable = yes
   public = yes
   writable = yes
   ```

4. **Create a Samba password for qbittorrent**
   ```bash
   sudo smbpasswd -a qbittorrent
   ```

5. **Restart Samba**
   ```bash
   sudo systemctl restart smbd
   ```

---

## ✅ Summary

- ✅ **Plex** is ready to stream your media collection.
- ✅ **qBittorrent** can download torrents directly into your Plex library.
- ✅ Files are shareable over your local network using **Samba**.
- ✅ Everything is configured to run automatically on boot.

---

> 📝 Tip: You can add additional drives and mount points for extended storage, and secure remote access with a VPN or reverse proxy.
