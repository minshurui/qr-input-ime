# QR Input IME v1.5 (二维码输入法)

An **Android IME** that lets you type from your phone onto your **TV box over the local network**.
Scan the QR code / open the console to send text, files, and images to the TV in real time — it even doubles as a remote control.

> Package: `com.qrinput.ime` ｜ Version: v1.5 ｜ Size: ~3.7 MB

**Read this in [中文](README.md).**

## ✨ Features

| Category | Feature |
|---|---|
| ⌨️ Input | Real-time mode (typing syncs instantly via incremental diff) & manual mode (send-all-at-once), delete / clear / confirm |
| 🎮 Remote | D-pad + OK / Back / Esc / Tab / Info / delete one char / Enter |
| 📜 Clipboard | Paste from phone & send, copy TV selection back to phone, last 10 history items |
| ⭐ Phrases | Custom shortcuts / URLs / accounts, one-tap fill |
| 🎤 Voice | Web Speech API (requires Chrome/Safari), zh-CN recognition |
| 📄 Files | Send txt / json / srt / m3u / xml / conf text files straight to the TV |
| 🖼 Image cast | Downscale to ≤1920px and send as JPEG to the TV |
| 📺 Multi-device | Add multiple boxes (default port 8765), switch with a dropdown, online status polling |

## 📲 Installation

1. Download `二维码输入法-v1.5.apk` from [Releases](../../releases) and install it on your Android phone;
2. Settings → Language & Input → enable **QR Input IME**;
3. Phone and box must be on the **same LAN**;
4. Run the companion server on your box (HTTP port `8765`) and show the QR code / console URL on the TV;
5. Scan the QR code with your phone → pick the device → start typing.

## 🔌 Box-side API (port 8765)

The console talks to the box over these REST endpoints (phone and box must be on the same subnet):

| Method | Path | Description |
|---|---|---|
| GET | `/api/status` | Device online status `{connected: bool}` |
| POST | `/api/sync` | Full text sync (replace the TV's input field) |
| POST | `/api/input` | Append input text |
| POST | `/api/delete` | Delete one character |
| POST | `/api/clear` | Clear the TV input field |
| POST | `/api/enter` | Press Enter |
| POST | `/api/key` | Send key (up/down/left/right/ok/back/esc/tab/info) |
| GET | `/api/selection` | Read the selected text on the TV |
| POST | `/api/image` | Cast an image `{data: "data:image/jpeg;base64,..."}` |

## 🛠 Self-host the console

`assets/input.html` is the console page bundled inside the APK (pure HTML+JS, zero dependencies).
You can host it directly on the box, or fork and repackage it.

## 📁 Repo layout

```
.
├── 二维码输入法-v1.5.apk   # Installer
├── assets/
│   └── input.html          # Bundled console source (self-hostable)
└── README.md / README.en.md
```

## 📄 License

[MIT](LICENSE) © minshurui — free to use, modify, distribute (incl. commercial), provided the copyright notice is retained.
