# 🎥 Raspberry Pi Home Media Server

This project turns your Raspberry Pi into a **powerful media server** that streams content to any device on your local network using either **Plex** or **Jellyfin**, with optional torrent integration using **qBittorrent**.

---

## 📦 Features

- 🔹 Full media server setup with **Plex** or **Jellyfin**
- 🔹 Automatic torrent downloads into the media library using **qBittorrent**
- 🔹 Optional local network file sharing with **Samba**
- 🔹 Hardware acceleration enabled for smoother playback/transcoding
- 🔹 Stream to any device on the network, including **TV sticks like Roku**, smartphones, tablets, or PCs

---

## 🆚 Plex vs Jellyfin

| Feature               | Plex                               | Jellyfin                             |
|----------------------|------------------------------------|--------------------------------------|
| Open Source          | ❌ Closed-source (free version)    | ✅ Fully open-source                 |
| Hardware Acceleration| ✅ Yes                             | ✅ Yes (manual configuration)       |
| Remote Access        | ✅ With subscription (Plex Pass)   | ✅ Free with manual setup            |
| Roku App             | ✅ Official support                | ✅ Community-supported               |
| Easy Setup           | ✅ Very easy                       | ⚠️ Slightly more technical           |

> ⚠️ As of April 2025, **remote streaming on Plex requires a paid Plex Pass** or a Remote Watch Pass.

---

## 🌐 How to Connect and Stream

### Jellyfin

1. **First-time setup**:  
   Open your browser and go to:  
   `http://<Your_Pi_IP>:8096`  
   Follow the on-screen instructions to create a **Jellyfin user account** and add your media libraries.

2. **Devices**:  
   Install the **Jellyfin app** on:
   - Roku (via channel store or Jellyfin channel sideload)
   - Android TV / Fire TV / Mobile
   - Web browsers: Chrome, Firefox, Edge, etc.

### Plex

1. **First-time setup**:  
   Open:  
   `http://<Your_Pi_IP>:32400/web`  
   Sign in or create a **Plex account**, and follow setup steps to configure libraries.

2. **Devices**:  
   Download the official **Plex app** for:
   - Roku / Smart TVs
   - Android / iOS
   - Desktop apps or browser

---

## 📁 Local Network File Sharing (Optional)

If you enabled **Samba**, you can access the media files from Windows or other devices via:
```
\<Your_Pi_IP>\Media
```
Use the Samba credentials you created (e.g., user: `pi` or `qbittorrent`).

---

## 🛠️ Files Included

- `RaspberryPi_MediaServer_Guide.md` – Setup guide using Plex
- `RaspberryPi_Jellyfin_qBittorrent_Guide.md` – Setup guide using Jellyfin with hardware acceleration and qBittorrent

---

## 💡 Tips

- Make sure your Raspberry Pi is on a wired connection for better streaming performance.
- You can access these services only on your local network unless you set up a reverse proxy or VPN.

---

## 📃 License

This project is open for personal and educational use. Attribution is appreciated.
