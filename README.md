<h1 align="center">
  <img src="docs/rustcord.png" width="120" alt="RustCord logo" />
  <br />
  RustCord <sup>BETA</sup>
</h1>

<p align="center">
  <strong>A lightweight Discord client built in Rust.</strong><br />
  Vencord and Shelter included. Themes, plugins, multiple profiles, and a tiny memory footprint.
</p>

<p align="center">
  <img alt="Version" src="https://img.shields.io/badge/version-7.0.1-blue" />
  <img alt="Status" src="https://img.shields.io/badge/status-beta-orange" />
  <img alt="Platform" src="https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-0078D4" />
  <img alt="Built with Rust" src="https://img.shields.io/badge/built%20with-Rust%20%2B%20Tauri-dea584" />
  <img alt="License" src="https://img.shields.io/badge/license-GPL--3.0-green" />
</p>

<p align="center">
  <a href="https://github.com/DaniDema/RustCord/releases/latest">
    <img alt="Download RustCord BETA" src="https://img.shields.io/badge/Download-RustCord%20BETA-ff7a18?style=for-the-badge&logo=windows&logoColor=white" />
  </a>
</p>

<p align="center">
  Made by <a href="https://k1wi.xyz">DaniDema (k1wi.xyz)</a> · Based on Legcord
</p>

---

## What is RustCord?

**RustCord** is a lightweight alternative Discord client for Windows, written in **Rust** and powered by **Tauri + WebView2**. It uses the system webview instead of shipping a full Chromium runtime, so it starts fast and uses far less RAM than Electron-based clients.

RustCord is built on **[Dorion](https://github.com/SpikeHD/Dorion)** by SpikeHD, follows the spirit of **[Legcord](https://github.com/Legcord/Legcord)**, and bundles the client-mod experience people expect:

- **Vencord** enabled by default
- **Shelter** plugin loader included
- Themes, plugins, keybinds, tray support, notifications, streamer mode, push-to-talk, and more

> **BETA:** RustCord is an early custom build. Core chat works, but some integrations and the online updater are still being polished. Expect rough edges.

## Features

| Feature | Description |
| --- | --- |
| **Rust + Tauri** | Native Rust backend with Microsoft WebView2. No bundled Chromium. |
| **Low RAM usage** | Roughly **60–100 MB** in normal use, depending on Discord and loaded plugins. |
| **Vencord by default** | Vencord is installed and enabled automatically on first launch. |
| **Shelter support** | Shelter is included for additional plugins and customization. |
| **Themes & plugins** | Customize the client with Vencord and Shelter themes/plugins. |
| **Multiple profiles** | Keep separate sessions and settings for different accounts. |
| **Desktop integration** | Tray icon, native notifications, global keybinds, streamer mode, PTT, and `discord://` links. |
| **Built-in updater** | Checks the project's GitHub releases for new RustCord versions. |
| **Portable friendly** | Dorion's portable mode is preserved for advanced users. |

## Download

The current beta release is a **Windows x64 MSI**:

```text
RustCord_7.0.1_DaniDema_x64_en-US.msi
```

**Download:** [Latest release](https://github.com/DaniDema/RustCord/releases/latest)

> **Publishing note:** the link above starts working once the first GitHub release is published and the MSI from your local `dist` folder is attached as a release asset.

SHA-256 of the `7.0.1` build:

```text
C11D12DA12E71E50A7C86E95D2BABAFA28823DBB07C5452D877D0E9C18AEF804
```

### System requirements

- Windows 10 or Windows 11 (64-bit)
- Microsoft Edge WebView2 Runtime (preinstalled on Windows 11; Windows 10 may need it)
- About 100 MB of free disk space

## Installation

1. Download the MSI from the [latest release](https://github.com/DaniDema/RustCord/releases/latest).
2. Run the installer.
3. If Windows SmartScreen appears, click **More info → Run anyway**. The beta MSI is **not code-signed** yet.
4. Launch **RustCord** from the Start Menu or desktop shortcut.

The installer creates:

- `C:\Program Files\RustCord\RustCord.exe`
- `C:\Program Files\RustCord\updater.exe`
- Start Menu and desktop shortcuts named **RustCord**

### First launch

- RustCord opens Discord's web app inside WebView2.
- Vencord and Shelter load automatically.
- Vencord may download its latest dev build on first start, so an internet connection is required.
- Configuration is stored in `%APPDATA%\dorion` to preserve compatibility with the upstream project.

> **Account safety:** Third-party Discord clients are technically against Discord's Terms of Service. RustCord is a modified open-source client, not an official Discord product. Use it at your own risk.

## Updates

RustCord checks:

```text
https://api.github.com/repos/DaniDema/RustCord/releases/latest
```

The updater is already pointed at this repository and will never install upstream Dorion. It will only find updates after releases are published on `DaniDema/RustCord`. Each release **must include an `.msi` asset** (for example `RustCord_7.0.1_DaniDema_x64_en-US.msi`); the updater downloads the first `.msi` it finds. Until then, update manually by installing a newer MSI.

## Build from source

### Prerequisites

- [Rust](https://rustup.rs/) with the MSVC toolchain
- [Node.js](https://nodejs.org/) 20+ and [pnpm](https://pnpm.io/)
- Visual Studio Build Tools with the **Desktop development with C++** workload
- Microsoft Edge WebView2 Runtime
- WiX Toolset v3 (Tauri can download it automatically during MSI bundling)

### Windows build

```powershell
git clone https://github.com/DaniDema/RustCord.git
cd RustCord

pnpm install

# Build the updater helper
cargo build --release --manifest-path updater/Cargo.toml
Copy-Item updater\target\release\updater.exe src-tauri\updater.exe -Force

# Build the app and MSI
pnpm tauri build --bundles msi --target x86_64-pc-windows-msvc
```

The MSI is written to:

```text
src-tauri\target\x86_64-pc-windows-msvc\release\bundle\msi\RustCord_7.0.1_x64_en-US.msi
```

### Development

```powershell
pnpm install
pnpm dev
```

## Project layout

```text
.
├── src/                  # Splash screen and frontend assets
├── src-tauri/            # Rust backend, Tauri config, icons, injection
│   ├── injection/        # Pre/post-inject scripts (Vencord, Shelter, UI)
│   ├── src/              # Rust source
│   └── tauri.conf.json   # Product name, identifier, bundle metadata
├── updater/              # Separate Rust updater binary
├── docs/                 # GitHub Pages landing page and assets
├── dist/                 # Local build output: MSI, logo, notes (gitignored)
└── RustCord.png          # Original logo
```

## Credits

RustCord would not exist without these projects:

- **[Dorion](https://github.com/SpikeHD/Dorion)** by **SpikeHD** — the Rust/Tauri foundation this project is built on. Thank you!
- **[Legcord](https://github.com/Legcord/Legcord)** — inspiration for the lightweight, mod-friendly Discord client experience.
- **[Vencord](https://github.com/Vendicated/Vencord)** — the client mod bundled by default.
- **[Shelter](https://github.com/uwu/shelter)** — plugin loader included with the client.
- **Discord** — the platform this client connects to.

**RustCord branding, packaging, and customization:** DaniDema · [k1wi.xyz](https://k1wi.xyz)

## License

RustCord is distributed under the **GNU General Public License v3.0**, inheriting the license of the Dorion project. See [LICENSE](LICENSE) for the full text.

Upstream components (Dorion, Legcord, Vencord, Shelter) remain under their respective licenses and copyrights.

## Disclaimer

RustCord is **not affiliated with, endorsed by, or sponsored by Discord Inc., Dorion, Legcord, Vencord, or Shelter**. Discord is a trademark of Discord Inc.

Using a third-party client may violate Discord's Terms of Service. The authors take no responsibility for account restrictions or bans. Use at your own risk.
