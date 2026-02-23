# Minecraft Bedrock on Linux

The complete guide to every known way to run **Minecraft Bedrock Edition** on Linux.

---

## ⚠ Critical Dependency Notice

All Android-based methods (Methods 1–5) rely on:

- `mcpelauncher-client`
- `mcpelauncher-extract`
- the **mcpelauncher-manifest** project

If that project disappears, these methods stop working — unless you already backed up the binaries.

---

## Overview of Methods

| # | Method | Source | Difficulty | Notes |
|---|--------|--------|-----------|------|
| 1 | mcpelauncher | Android | Easy | Official & legal |
| 2 | Trinity Launcher | Android | Easy | Modern GUI + features |
| 3 | Cianova Launcher | Android | Easy | Python GUI, hackable |
| 4 | Iosel Edition | Android | Easy | AppImage fork |
| 5 | Raw CLI | Android | Medium | Minimal & reliable |
| 6 | ProtonGDK | Windows PC | Hard | Runs actual PC version |
| 7 | Windows VM | Windows PC | Easy | Heavy but reliable |

---

## Method 1 — mcpelauncher

The original launcher and long-standing standard.

Requires owning Minecraft on Google Play. It downloads the APK using your account.

> ⚠ Maintainer ChristopherHX stated:
> *"Yes we are sitting on top a cave full of tnt, you have no long term update warranties. Update delays / stops could happen at any point of time, since the DRM has been enabled."*

**Key Points**

- ✔ Legal — uses your purchased copy  
- ✔ Automatic APK download via Google login  
- ✖ Custom APKs not officially supported  
- ✔ First launcher to run Bedrock on Linux  

Repo: https://github.com/minecraft-linux/mcpelauncher-manifest  
Docs: https://mcpelauncher.readthedocs.io/en/latest/getting_started/index.html  
Tutorial: https://youtu.be/rg_OH-nmoeQ  

---

## Method 2 — Trinity Launcher

A polished, actively maintained launcher with extended features.

**Features**

- Uses `mcpelauncher-client` & `mcpelauncher-extract`
- Supports custom APKs
- Version management & export/import
- Mod & texture pack management
- Discord Rich Presence
- Built with C++ & Qt6

Repo: https://github.com/Trinity-LA/Trinity-Launcher  
Docs: https://trinitylauncher.vercel.app/  
Tutorial: https://youtu.be/ZMAamMBm8Go  

---

## Method 3 — Cianova Launcher

Community-made launcher with a Python GUI using **customtkinter**.

**Highlights**

- Uses mcpelauncher utilities
- Supports custom APKs
- Easier to read & modify than Trinity
- Nvidia Prime, GameMode & Zink support
- Dependency checker included

Repo & Docs: https://github.com/PlaGaPlusDev/CianovaLauncher-mcpelauncher  
Tutorial: https://youtu.be/-tl4ZSJ3DSE  

---

## Method 4 — Iosel Edition

A modified **AppImage** fork of mcpelauncher.

**Highlights**

- Standalone AppImage
- No Flatpak runtime required
- Custom APK configuration support
- Developed by a contributor associated with Trinity

Repo & Docs: https://github.com/IoselDev/Minecraft-Bedrock-Linux-Iosel-Edition  
Tutorial: https://youtu.be/BAZqN_NhIj4  

---

## Method 5 — Raw CLI (recommended)

Skip the launchers entirely. Use mcpelauncher's utilities directly from the terminal. This is what every launcher above does under the hood — two commands and you're playing.

### Step 1 — Install mcpelauncher-client

```sh
curl -fsSL https://minecraft-linux.github.io/pkg/deb/pubkey.gpg \
  | gpg --dearmor \
  | sudo tee /etc/apt/trusted.gpg.d/mcpelauncher.gpg > /dev/null

echo "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mcpelauncher.gpg] \
  https://minecraft-linux.github.io/pkg/deb noble main" \
  | sudo tee /etc/apt/sources.list.d/mcpelauncher.list

sudo apt update
sudo apt install mcpelauncher-manifest
```

### Step 2 — Build and install mcpelauncher-extract

Not in the repo as a separate package — compile it yourself (takes under a minute):

```sh
sudo apt install git cmake clang libzip-dev

git clone https://github.com/minecraft-linux/mcpelauncher-extract.git
cd mcpelauncher-extract
mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

### Step 3 — Backup the binaries

Both are GPL licensed — you can copy and redistribute freely. Back them up in case the repo goes down:

```sh
cp /usr/bin/mcpelauncher-client ~/mcpelauncher-client.bak
cp /usr/local/bin/mcpelauncher-extract ~/mcpelauncher-extract.bak
```

### Step 4 — Get a Minecraft APK

Download the **x86_64** APK from [mcpelife.com](https://mcpelife.com). Make sure it's x86_64, not ARM.

### Step 5 — Extract and play

```sh
# extract (once per version)
mcpelauncher-extract ~/Downloads/minecraft-1.26.1.1-x86_64.apk \
  ~/.local/share/mcpelauncher/versions/1.26.1.1

# play
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/1.26.1.1
```

To install a different version, repeat Step 5 with a different APK and version folder name.

### NOTE
`mcpelauncher-client` only works with x86_64 APKs. Such APKs are very hard to find. Cracked x86_64 APKs are available at [mcpelife.com](https://mcpelife.com). But I couldn't find any reliable way to get x86_64 APK for the legal version of MCPE. If you manage to do so, that should still work.

### Restoring from backup
If the repo goes down and you need to restore the binaries you backed up:
```bash
shcp ~/mcpelauncher-client.bak /usr/local/bin/mcpelauncher-client
cp ~/mcpelauncher-extract.bak /usr/local/bin/mcpelauncher-extract
chmod +x /usr/local/bin/mcpelauncher-client
chmod +x /usr/local/bin/mcpelauncher-extract
```

Then use them exactly as normal:
```bash
shmcpelauncher-extract ~/Downloads/minecraft.apk ~/.local/share/mcpelauncher/versions/1.26.1.1
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/1.26.1.1
```
---

## Method 6 — GDK version via ProtonGDK

Minecraft Bedrock has shifted from UWP to GDK on Windows. This method runs the actual PC GDK version on Linux using ProtonGDK and some proxy workarounds.

- Runs the actual Windows PC version, not Android
- Lengthy setup — not recommended unless other methods fail
- No specific repo — combination of multiple tools and workarounds

[Video Tutorial](https://youtu.be/m76O2cRIEnM?si=YV2dxJpvLTYKtIDh)
[Textual Tutorial](https://github.com/inbob1/mcbe-on-linux)

---

## Method 7 — Windows VM

Run Windows in a virtual machine and play Minecraft Bedrock normally inside it. Last resort — works but has overhead and requires a Windows license.

---

## Outdated Methods

### Waydroid

Ran a full Android environment and played Minecraft like on a phone. Stopped working in newer versions — game gets stuck at the loading screen.

### CCMC Launcher by Crow_Rei34

Worked similarly to Trinity — extracted and ran custom APKs. Repository has since been deleted or privated. Believed to have been discontinued when Trinity was introduced, as both share a developer (JavierC).

Historical tutorials (Portugese): [1](https://youtu.be/4K1X8PafWf0?si=jZe4X5xsA9nplT5h) | [2](https://youtu.be/VfVwqMcKhy0?si=_C70rhEyabASEKgb) | [3](https://youtu.be/sYvpEpwin6k?si=sD631OfATfYwzbH1)
