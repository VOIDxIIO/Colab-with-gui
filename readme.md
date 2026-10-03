🖥️ Colab Desktop

Turn Google Colab into a Linux desktop you can control from your phone or PC. Free. No VPS needed.

---

✨ What You Get

· 🖥️ XFCE Linux Desktop running in the cloud
· 🦀 RustDesk remote access from any device
· 🌐 Google Chrome (optional)
· 🎮 Steam GUI client (optional)
· ⏰ Keep-alive script to stop Colab from disconnecting

---

🚀 How to Use

1. Open the Notebook

Click the badge or open colab_desktop.ipynb in this repo.
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/VOIDxIIO/Colab-with-gui/blob/main/Colab-with-gui.ipynb)

2. Set Your Password

In the first cell, change colab1234 to your own password.

3. Run the Cell

Press ▶️ and wait about 3–5 minutes while everything installs.

4. Get Your ID

When it finishes, you'll see:

```
=======================================================

✅ YOUR COLAB DESKTOP IS READY

=======================================================

RustDesk ID : 1234567890
Password : colab1234

=======================================================
```

Copy the RustDesk ID.

5. Connect From Your Phone

1. Install RustDesk from the App Store or Play Store
2. Open it, enter the ID
3. Enter your password
4. You're in

---

🎯 How to Open Apps

Important: You must launch all apps from the Terminal. The desktop menu icons won't work because Colab runs as root and the shortcuts don't include the required flags.

How to Open Terminal

· Click Applications (top-left) → Terminal Emulator
· Or press Ctrl + Alt + T

Google Chrome

Do NOT try to open it from the Applications menu. It won't work.

Open the Terminal and type:

```
DISPLAY=:99 google-chrome --no-sandbox --disable-dev-shm-usage &
```

Steam

Do NOT try to open it from the Applications menu. It won't work.

Open the Terminal and type:

```
DISPLAY=:99 /usr/games/steam --no-sandbox &
```

First launch takes a few minutes while Steam updates itself. Be patient.

File Manager

Open the Terminal and type:

```
DISPLAY=:99 thunar &
```

Any Other App

Always launch from the Terminal like this:

```
DISPLAY=:99 <app-name> --no-sandbox &
```

· DISPLAY=:99 tells it which screen to use
· --no-sandbox is needed because Colab runs as root
· & runs it in the background

If it says "command not found", run this once:

```
export PATH="$PATH:/usr/games"
```

---

⚠️ Important Limits

· Colab sessions last 9–12 hours max
· When it resets, everything is wiped and your ID changes
· Just re-run the cell to rebuild
· Keep the Colab tab open (minimized is fine) or it disconnects
· 3D games won't work — no GPU on the virtual display
· This is a fun project, not a permanent server

---

🛠️ Troubleshooting

RustDesk ID is empty?
Check logs:

```
!cat /content/rd_server.log
```

Then re-run the cell.

Black screen after connecting?
Restart XFCE:

```
pkill -9 xfce4
DISPLAY=:99 startxfce4 &
```

Session keeps disconnecting?
Keep the Colab tab open. The keep-alive script clicks "Connect" every 60 seconds.

---

🧠 How It Works

1. Xvfb creates a fake virtual screen
2. XFCE runs a desktop on it
3. RustDesk streams that screen to your device
4. Keep-alive JS prevents Colab from idling out

---

📜 License

MIT — do whatever you want.

---

⭐ Credits

· RustDesk
· XFCE
· Google Colab

---

🐛 Found a Bug?

Open an issue with your Colab output and device type.

⚠️ Disclaimer: Educational project only. Don't use against Colab's ToS.
