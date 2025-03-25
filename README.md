# Brain.fm Desktop App

An Electron wrapper for the Brain.fm web application that provides a clean, dedicated application experience.

## Features

- Runs Brain.fm in a dedicated window without browser controls
- Clean interface focused on music playback
- Seamless integration with your desktop environment
- Global media key support (play/pause/skip)

## Installation

### Prerequisites
- Node.js (v14+)
- npm (v6+)
- Git

### Method 1: Build from Source

1. Clone this repository:
```bash
git clone git@github.com:clarkezyz/brainfm-electron.git
cd brainfm-electron
```

2. Install dependencies:
```bash
npm install
```

3. Run the app:
```bash
npm start
```

### Method 2: Install Pre-built Package

#### AppImage (Recommended)
1. Download the latest AppImage from the releases page
2. Make it executable:
```bash
chmod +x BrainFM-*.AppImage
```
3. Extract it (necessary due to sandbox permissions):
```bash
./BrainFM-*.AppImage --appimage-extract
```
4. Create a desktop shortcut:
```bash
cat > ~/.local/share/applications/brainfm-app.desktop << EOF
[Desktop Entry]
Name=Brain.fm
Comment=Brain.fm Desktop App
Exec=$(realpath /path/to/squashfs-root/zyzd-brainfm) --no-sandbox
Terminal=false
Type=Application
Categories=Audio;Music;
Icon=$(realpath /path/to/build/icons/256x256.png)
StartupWMClass=zyzd-brainfm
EOF
```
5. Update desktop database:
```bash
update-desktop-database ~/.local/share/applications
```

#### Debian Package
```bash
sudo dpkg -i brainfm_*.deb
sudo apt-get -f install  # To resolve any dependencies
```

## Building from Source

To build the app into distributable formats:

```bash
# Install development dependencies
npm install --save-dev electron-builder

# Build for Linux
npm run build
```

This will create AppImage and .deb packages in the `dist` directory.

### Building with Custom Icons

If you want to use custom icons:

1. Place your icons in the `build/icons` directory with the following sizes:
   - 16x16.png
   - 32x32.png
   - 64x64.png
   - 128x128.png
   - 256x256.png
   - 512x512.png

2. Update the package.json "build" section if needed:
```json
"build": {
  "appId": "com.your-name.brainfm",
  "productName": "BrainFM",
  "linux": {
    "target": ["AppImage", "deb"],
    "category": "Audio",
    "icon": "build/icons",
    "maintainer": "Your Name <your.email@example.com>"
  }
}
```

## Configuration

You can modify the `main.js` file to customize the app's behavior:

```javascript
// Change the default window size
const win = new BrowserWindow({
  width: 1200,
  height: 800,
  // Other options...
})

// Enable devtools in development
if (process.env.NODE_ENV === 'development') {
  win.webContents.openDevTools()
}
```

## Troubleshooting

### Sandbox Issues
If you encounter sandbox-related errors, run the application with the `--no-sandbox` flag:
```bash
./BrainFM-*.AppImage --no-sandbox
```

### AppImage Extraction
If FUSE is not available on your system, you can extract the AppImage:
```bash
./BrainFM-*.AppImage --appimage-extract
./squashfs-root/zyzd-brainfm --no-sandbox
```

### Audio Issues
If you encounter audio issues:
1. Make sure your system's audio is working properly
2. Try restarting PulseAudio:
   ```bash
   pulseaudio -k
   pulseaudio --start
   ```

## License

MIT
