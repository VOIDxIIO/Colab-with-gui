🖥️ Colab Desktop

Turn Google Colab into a full Linux desktop you can control from your phone, tablet, or PC — for free.

This project installs a complete XFCE desktop environment, RustDesk remote access, Google Chrome, and Steam inside a Google Colab notebook. No VPS, no credit card, no local setup.

---

📑 Table of Contents

· Features
· Quick Start
· How to Open Apps
· Configuration
· Limitations
· Troubleshooting
· How It Works
· License

---

✨ Features

· 🖥️ Full XFCE Linux desktop running in the cloud
· 🦀 RustDesk remote access — connect from iOS, Android, Windows, macOS, or Linux
· 🌐 Google Chrome pre-installed and pre-configured
· 🎮 Steam GUI client ready to launch
· ⏰ Keep-alive script prevents idle disconnects
· 🔓 One-click setup — no terminal knowledge needed

---

🚀 Quick Start

1. Open in Colab

Click the badge below (or open colab_desktop.ipynb from this repo):

https://colab.research.google.com/assets/colab-badge.svg

Replace YOUR_USERNAME/YOUR_REPO with your GitHub path.

2. Set Your Password

In the first cell, replace the default password colab1234 with your own.

3. Run the Cell

Press the ▶️ play button. Wait 3–5 minutes.

4. Copy Your ID

When the cell finishes, you'll see:

```text
=======================================================

=======================================================

✅ YOUR COLAB DESKTOP IS READY

=======================================================

=======================================================

RustDesk ID : 1234567890
Password : colab1234

=======================================================

=======================================================
```

Copy the RustDesk ID (the 10-digit number).

5. Connect From Your Device

1. Install RustDesk on your phone or PC:
   · Google Play
   · App Store
   · Windows / macOS / Linux
2. Open the app → enter the RustDesk ID
3. Enter your password
4. You're on your cloud desktop.

---

🎯 How to Open Apps

You're now looking at the XFCE desktop. Here's how to launch every app, both from the GUI menu and the terminal.

General Rule for Any App

If a shortcut doesn't work, always launch from the terminal:

```bash
DISPLAY=:99 <app-name> --no-sandbox &
```

· DISPLAY=:99 → points to our virtual screen (required)
· --no-sandbox → bypasses root restriction (Colab runs as root)
· & → runs it in the background

If an app says "command not found", it's usually in /usr/games which isn't in $PATH. Fix once:

```bash
export PATH="$PATH:/usr/games"
source ~/.bashrc
```

1. Terminal

GUI menu: Applications → Terminal Emulator

Keyboard shortcut: Ctrl + Alt + T

Right-click desktop: Open Terminal Here

2. File Manager

GUI menu: Applications → File Manager

Desktop icon: Double-click the Home folder icon

Terminal:

```bash
DISPLAY=:99 thunar &
```

Useful paths:

· /content/ → Colab's main writable directory
· /root/ → Your home folder
· /usr/share/applications/ → Where app launchers live

3. Google Chrome

GUI menu: Applications → Internet → Google Chrome

The setup script patched the launcher to include --no-sandbox, so it just works.

Terminal:

```bash
DISPLAY=:99 google-chrome --no-sandbox --disable-dev-shm-usage --disable-gpu &
```

Desktop shortcut:

· Right-click desktop → Create Launcher
· Name: Chrome
· Command: google-chrome --no-sandbox --disable-dev-shm-usage --disable-gpu

⚠️ Expect it to be slow. Every frame is streamed from Colab to your phone. For heavy browsing, use your phone's own browser.

4. Steam

GUI menu: Applications → Games → Steam

If it's not there, create the shortcut manually (run once):

```bash
cat > /usr/share/applications/steam.desktop <<EOF
[Desktop Entry]
Name=Steam
Exec=/usr/games/steam --no-sandbox %U
Icon=steam
Type=Application
Categories=Game;
EOF
update-desktop-database
```

Terminal (most reliable):

```bash
DISPLAY=:99 /usr/games/steam --no-sandbox &
```

Why the full path? steam is installed at /usr/games/steam, which isn't in $PATH by default.

First launch takes 3–10 minutes — Steam downloads its client files. Don't kill it if the window appears blank.

5. Any Other App

Step 1 — Find the binary:

```bash
which <app-name>
ls /usr/bin/ | grep <app-name>
ls /usr/games/ | grep <app-name>
```

Step 2 — Launch with display:

```bash
DISPLAY=:99 <full-path-to-binary> --no-sandbox &
```

Step 3 — If it's Electron (VS Code, Discord, Slack, Obsidian):

```bash
DISPLAY=:99 <app> --no-sandbox --disable-gpu --disable-dev-shm-usage &
```

Step 4 — Add a desktop shortcut:

```bash
sudo nano /usr/share/applications/myapp.desktop
```

Paste:

```ini
[Desktop Entry]
Name=My App
Exec=/path/to/binary --no-sandbox
Icon=myapp
Type=Application
Categories=Utility;
```

Save, then:

```bash
sudo update-desktop-database
```

🔧 Bonus: Launch Multiple Apps at Once

Create a startup script:

```bash
cat > /root/.config/autostart.sh <<'EOF'
#!/bin/bash
export DISPLAY=:99
google-chrome --no-sandbox --disable-dev-shm-usage &
thunar &
xfce4-terminal &
EOF
chmod +x /root/.config/autostart.sh
```

Then add it to XFCE:

· Applications → Settings → Session and Startup → Application Autostart → Add
· Name: Custom Apps
· Command: /root/.config/autostart.sh

---

⚙️ Configuration

At the top of the Colab cell, you can toggle options:

Option Default Description
RUSTDESK_PASSWORD colab1234 Your connection password
INSTALL_CHROME True Install Google Chrome
INSTALL_STEAM True Install the Steam GUI client

Set to False to skip a component (faster setup).

---

⚠️ Important Limitations

🕐 Session Lifetime

Colab is NOT a permanent server.

· Idle timeout: ~90 minutes (keep-alive prevents this if the tab stays open)
· Hard limit: ~9–12 hours per session
· After reset: All apps wiped, RustDesk ID changes

🔄 After a Reset

Re-run the Colab cell. Everything rebuilds in ~3 minutes. Grab the new ID and reconnect.

🚫 What Won't Work

· 3D games — no GPU on the virtual display
· Snap apps — no snapd daemon in Colab
· Heavy workloads — Colab isn't a VPS

💡 For a Permanent Setup

Use a real VPS:

· Oracle Cloud Free Tier — free forever, 4 CPU / 24GB RAM
· DigitalOcean — $5/month
· Hetzner — €4/month

Same script works — just remove the keep-alive JavaScript.

---

🛠️ Troubleshooting

"Command not found" for an app:

```bash
export PATH="$PATH:/usr/games"
source ~/.bashrc
```

RustDesk ID is empty:

```bash
!cat /content/rd_server.log
```

Then re-run the cell.

Chrome won't open from the menu:

```bash
DISPLAY=:99 google-chrome --no-sandbox --disable-dev-shm-usage &
```

Session disconnected:
Keep the Colab tab open (minimized is fine). The keep-alive JavaScript clicks "Connect" every 60 seconds.

Steam login hangs:
First launch downloads ~500MB. Wait 5–10 min. Check:

```bash
cat ~/.steam/error.log
```

Black screen after connecting via RustDesk:

```bash
pkill -9 xfce4
DISPLAY=:99 startxfce4 &
```

"Unable to open display":
You forgot DISPLAY=:99 — always include it when launching GUI apps.

---

🧠 How It Works

1. Xvfb creates a virtual X11 display (:99) — a fake monitor
2. XFCE runs on that display — a full desktop environment
3. RustDesk streams that display to your device
4. Keep-alive JavaScript clicks "Connect" every 60s to prevent Colab from idling out

All processes use subprocess.Popen(..., start_new_session=True) to survive the parent shell exiting.

---

📜 License

MIT — do whatever you want with it.

---

⭐ Credits

· RustDesk — open-source remote desktop
· XFCE — lightweight Linux desktop
· Google Colab — free cloud compute

---

🐛 Found a Bug?

Open an issue with:

1. Your Colab output
2. Your device (Android/iOS/Windows/Mac)
3. Any error messages

---

⚠️ Disclaimer: Educational project only. Google Colab's ToS may change. Don't use for production or anything against Colab's ToS.
