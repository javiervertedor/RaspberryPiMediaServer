# 🏠 Raspberry Pi Home Media Server Setup with Jellyfin (Hardware Acceleration)

This guide shows you how to install and configure **Jellyfin** on a Raspberry Pi, including **hardware acceleration** for smoother media playback and transcoding. Jellyfin is a fully open-source alternative to Plex.

---

## 📡 Accessing Raspberry Pi via SSH (Windows)

1. **Install PuTTY**  
   Download and install [PuTTY](https://www.putty.org/) on your Windows PC.

2. **Connect via SSH**
   - Open PuTTY.
   - Enter your Raspberry Pi’s IP address in the **Host Name** field.
   - Set **Port** to `22` and **Connection type** to `SSH`.
   - Click **Open** and login with your Raspberry Pi username and password.

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

### 4. Access the Jellyfin Web Interface  
Open a browser and go to:  
`http://<Raspberry_Pi_IP>:8096`

---

## ⚙️ Enable Hardware Acceleration (Raspberry Pi)

### 1. Install Required Libraries
```bash
sudo apt install -y     libdrm2     libegl1-mesa     libgbm1     libgl1-mesa-dri     libgles2     mesa-va-drivers     mesa-vdpau-drivers     vainfo     vdpauinfo     intel-media-va-driver-non-free     ffmpeg
```

> **Note:** The `intel-media-va-driver-non-free` package is generally for x86; Raspberry Pi uses `v4l2` (Video for Linux 2) and `ffmpeg` compiled with Raspberry Pi support. Jellyfin can still use VAAPI and V4L2 for decoding.

### 2. Configure Jellyfin to Use Hardware Acceleration
1. Go to the Jellyfin dashboard.
2. Navigate to **Playback > Transcoding**.
3. Enable **Hardware acceleration**.
4. Select `Video Acceleration API (VAAPI)` or `Video4Linux2 (V4L2)` depending on the availability.

### 3. Add `jellyfin` User to Required Groups
```bash
sudo usermod -aG video jellyfin
sudo usermod -aG render jellyfin
```

Then reboot:
```bash
sudo reboot
```

---

## 📁 Setup Media Library Locations

1. Create media folders:
```bash
sudo mkdir -p /home/pi/Media/Movies
sudo mkdir -p /home/pi/Media/TV
```

2. Change ownership:
```bash
sudo chown -R jellyfin:jellyfin /home/pi/Media
```

3. Add these folders to your Jellyfin library in the web interface.

---

## 🔁 Optional: Samba Sharing for Local Network

1. **Install Samba**
```bash
sudo apt install samba
```

2. **Configure Samba**
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

3. **Set Samba password for pi**
```bash
sudo smbpasswd -a pi
```

4. **Restart Samba**
```bash
sudo systemctl restart smbd
```

---

## ✅ Summary

- ✅ **Jellyfin** is installed and running.
- ✅ **Hardware acceleration** is enabled using VAAPI/V4L2 for better performance.
- ✅ Media library is accessible from Jellyfin and optionally via local network using Samba.

---

> 💡 Tip: For remote access, consider setting up Jellyfin through a reverse proxy with HTTPS (e.g., using Nginx + Let's Encrypt).
