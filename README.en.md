# QR Input IME v1.5 (二维码输入法)

An **Android IME** that lets you type from your phone onto your **TV box over the local network**.
Scan the QR code / open the console to send text, files, and images to the TV in real time — it even doubles as a remote control.

> Package: `com.qrinput.ime` ｜ Version: v1.6 ｜ Size: ~3.4 MB

**Read this in [中文](README.md).**

## 📸 Preview

![Main console](docs/screenshot.png)

![Live typing sync demo](docs/typing.gif)

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

1. Install `二维码输入法-v1.6.apk` on an **Android TV box / TV** (any Android-based box);
2. Settings → Language & Input → enable **QR Input IME**;
3. A pairing QR code (console URL) appears on the TV;
4. **No app needed on your phone** — scan the QR code and the console opens in your browser.

> The server is built into the APK (NanoHTTPD, port 8765).

## 🔌 Built-in server API (port 8765)

The APK embeds an HTTP server that serves both the console page and the REST endpoints below (phone and box must be on the same subnet):

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
| GET | `/api/phrases` | Common phrases `{list: [...]}` |
| POST | `/api/phrases` | Save phrases (body: JSON array) |
| GET | `/api/history` | Clipboard history `{list: [...]}` |
| POST | `/api/history` | Save history (body: JSON array) |

> Phrases & history are stored on the box — data persists across phones.

## 📁 Repo layout

```
.
├── 二维码输入法-v1.6.apk   # Installer
├── assets/
│   └── input.html          # Bundled console source
├── docs/
│   ├── screenshot.png
│   └── typing.gif
├── README.md
└── README.en.md
```

## 📄 License

[MIT](LICENSE) © minshurui
