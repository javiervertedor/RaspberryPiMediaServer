# 🏠 Raspberry Pi Home Media Server Setup with Jellyfin & qBittorrent (Hardware Acceleration)

This guide shows how to set up **Jellyfin** with **hardware acceleration** and integrate **qBittorrent** for automated media downloads on a Raspberry Pi. Your media will be accessible locally via Samba and streamed through Jellyfin.

---

## 📡 Accessing Raspberry Pi via SSH (Windows)

1. **Install PuTTY**  
   Download and install [PuTTY](https://www.putty.org/) on your Windows PC.

2. **Connect via SSH**
   - Open PuTTY.
   - Enter your Raspberry Pi’s IP address.
   - Set **Port** to `22` and **Connection type** to `SSH`.
   - Click **Open**, then log in with your Pi's credentials.

---

## 🎞️ Installing Jellyfin with Hardware Acceleration

### 1. Update Your System
```bash
sudo apt update
sudo apt upgrade
```

### 2. Add Jellyfin Repository and Install
```bash
curl -fsSL https://repo.jellyfin.org/debian/jellyfin_team.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/jellyfin-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/jellyfin-archive-keyring.gpg] https://repo.jellyfin.org/debian bookworm main" | sudo tee /etc/apt/sources.list.d/jellyfin.list
sudo apt update
sudo apt install jellyfin
```

### 3. Enable and Start Jellyfin
```bash
sudo systemctl enable jellyfin
sudo systemctl start jellyfin
```

### 4. Access Web Interface  
Open:  
`http://<Raspberry_Pi_IP>:8096`

---

## ⚙️ Enable Hardware Acceleration

### 1. Install Required Libraries
```bash
sudo apt install -y     libdrm2     libegl1-mesa     libgbm1     libgl1-mesa-dri     libgles2     mesa-va-drivers     mesa-vdpau-drivers     vainfo     vdpauinfo     ffmpeg
```

### 2. Add Jellyfin User to Groups
```bash
sudo usermod -aG video jellyfin
sudo usermod -aG render jellyfin
sudo reboot
```

### 3. Enable in Jellyfin
- Go to **Dashboard > Playback > Transcoding**
- Enable **Hardware acceleration**
- Select **VAAPI** or **V4L2**

---

## 📂 Create and Configure Media Folders

1. **Create media directories**
```bash
sudo mkdir -p /home/pi/Media/Movies
sudo mkdir -p /home/pi/Media/TV
```

2. **Set permissions**
```bash
sudo chown -R jellyfin:jellyfin /home/pi/Media
sudo chmod -R 755 /home/pi/Media
```

---

## 🌐 Install and Configure qBittorrent

### 1. Install qBittorrent-nox
```bash
sudo apt install qbittorrent-nox
```

### 2. Create a qbittorrent user
```bash
sudo adduser --system --group qbittorrent
sudo mkdir /home/qbittorrent
sudo chown qbittorrent:qbittorrent /home/qbittorrent
sudo usermod -d /home/qbittorrent qbittorrent
```

### 3. Run qBittorrent manually (optional for testing)
```bash
sudo -u qbittorrent qbittorrent-nox
```

### 4. Create systemd service
```bash
sudo nano /etc/systemd/system/qbittorrent.service
```

Paste:
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

Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable qbittorrent
sudo systemctl start qbittorrent
```

### 5. Access Web UI  
Open:  
`http://<Raspberry_Pi_IP>:8080`  
Default login:  
- Username: `admin`  
- Password: `adminadmin`

---

## 🎬 Download Directly into Jellyfin Library

1. **Configure qBittorrent save path**
   - Go to: `Tools > Options > Downloads`
   - Set **Default Save Path** to:
     ```
     /home/pi/Media/Movies
     ```

2. **Ensure folder access**
```bash
sudo chown -R jellyfin:qbittorrent /home/pi/Media
sudo chmod -R 775 /home/pi/Media
```

> Note: Both `jellyfin` and `qbittorrent` users should be in the same group if needed.

---

## 📁 Optional: Network Share with Samba

1. **Install Samba**
```bash
sudo apt install samba
```

2. **Edit Samba config**
```bash
sudo nano /etc/samba/smb.conf
```

Add to the bottom:
```ini
[Media]
path = /home/pi/Media
available = yes
valid users = pi
read only = no
browsable = yes
public = yes
writable = yes
```

3. **Set Samba password**
```bash
sudo smbpasswd -a pi
```

4. **Restart Samba**
```bash
sudo systemctl restart smbd
```

---

## ✅ Summary

- ✅ **Jellyfin** is streaming your library.
- ✅ **Hardware acceleration** improves playback/transcoding.
- ✅ **qBittorrent** downloads directly into Jellyfin folders.
- ✅ **Samba** allows media sharing across devices.

---

> 💡 Tip: Use Jellyfin apps for Roku, Android TV, or browsers for easy access.
