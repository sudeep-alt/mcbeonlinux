# Minecraft Bedrock on Linux

The complete guide to every known way to run **Minecraft Bedrock Edition** on Linux.

---

## ⚠ Dependency Warning

All Android-based methods (Methods 1–5) depend on the same underlying toolchain:

- `mcpelauncher-client` — the runtime that executes the Android game binaries on Linux
- `mcpelauncher-extract` — extracts the game files from an APK
- The **mcpelauncher-manifest** project — coordinates installation and packaging of the above

> **If the mcpelauncher-manifest project is ever taken down or abandoned, Methods 1–5 will stop working** — unless you have already backed up the compiled binaries. Method 5 includes a step for doing exactly that.

---

## Overview of Methods

| # | Method | Source | Difficulty | Best For |
|---|--------|--------|------------|----------|
| 1 | mcpelauncher | Android APK | Easy | First-timers with a Google Play account |
| 2 | Trinity Launcher | Android APK | Easy | Users wanting full GUI feature set |
| 3 | Cianova Launcher | Android APK | Easy | Users who want a lightweight, readable launcher |
| 4 | Iosel Edition | Android APK | Easy | Users who prefer a self-contained AppImage |
| 5 | Raw CLI | Android APK | Medium | Power users; **recommended for reliability** |
| 6 | ProtonGDK | Windows PC | Hard | Users who specifically want the PC GDK build |
| 7 | Windows VM | Windows PC | Easy | Last resort; any hardware that can run a VM |

---

## Method 1 — mcpelauncher

The original launcher and long-standing community standard for running Bedrock on Linux.

Requires owning Minecraft on Google Play. The launcher authenticates with your Google account and downloads the APK directly — no manual APK hunting required.

> ⚠ **Maintainer's own warning (ChristopherHX):**
> *"Yes we are sitting on top a cave full of TNT, you have no long term update warranties. Update delays / stops could happen at any point of time, since the DRM has been enabled."*
>
> In plain terms: future Minecraft updates may break the launcher with little or no notice, and there is no guarantee it will be fixed promptly.

**Key Points**

- ✔ Legal — uses your own purchased copy via Google Play
- ✔ Automatic APK download via Google account login
- ✔ First launcher ever to run Bedrock on Linux
- ✖ Custom (sideloaded) APKs are not officially supported
- ✖ Long-term update reliability is not guaranteed due to DRM changes

**Links**

- [GitHub Repository](https://github.com/minecraft-linux/mcpelauncher-manifest)
- [Official Documentation](https://mcpelauncher.readthedocs.io/en/latest/getting_started/index.html)
- [Video Tutorial](https://youtu.be/rg_OH-nmoeQ)

---

## Method 2 — Trinity Launcher

A polished, actively maintained GUI launcher with a broad feature set built on top of the mcpelauncher toolchain. Suitable for users who want a full-featured experience without touching the command line.

**Features**

- Built on `mcpelauncher-client` and `mcpelauncher-extract`
- Supports custom APK sideloading
- Version management with export and import functionality
- Mod and texture pack management built in
- Discord Rich Presence integration
- Written in C++ with a Qt6 interface

**Links**

- [GitHub Repository](https://github.com/Trinity-LA/Trinity-Launcher)
- [Documentation & Website](https://trinitylauncher.vercel.app/)
- [Video Tutorial](https://youtu.be/ZMAamMBm8Go)

---

## Method 3 — Cianova Launcher

A community-made Python GUI launcher using the **customtkinter** library. Lighter and more readable than Trinity, making it a good option for users who may want to inspect or modify the launcher's source code.

**Highlights**

- Built on the same mcpelauncher utilities as other launchers
- Supports custom APK sideloading
- Codebase is easier to read and modify than Trinity's C++
- Nvidia Prime Offload support (for laptops with hybrid GPU setups)
- GameMode integration for performance optimization
- Zink (OpenGL-over-Vulkan) support
- Built-in dependency checker to catch missing packages before launch

**Links**

- [GitHub Repository & Documentation](https://github.com/PlaGaPlusDev/CianovaLauncher-mcpelauncher)
- [Video Tutorial](https://youtu.be/-tl4ZSJ3DSE)

---

## Method 4 — Iosel Edition

A modified fork of mcpelauncher distributed as a standalone **AppImage**. Useful if you want to avoid package managers and Flatpak runtimes entirely.

**Highlights**

- Packaged as a self-contained AppImage — download and run, no installation needed
- No Flatpak runtime or system package dependencies required
- Supports custom APK configuration
- Developed by a contributor with ties to the Trinity Launcher project

**Links**

- [GitHub Repository & Documentation](https://github.com/IoselDev/Minecraft-Bedrock-Linux-Iosel-Edition)
- [Video Tutorial](https://youtu.be/BAZqN_NhIj4)

---

## Method 5 — Raw CLI *(Recommended)*

Skip the launchers entirely and use the mcpelauncher toolchain directly from the terminal. This is exactly what every GUI launcher above does internally — reduced to two commands. Gives you full control, no GUI overhead, and makes backup and restoration straightforward.

---

### Step 1 — Install mcpelauncher-client

Add the upstream apt repository, then install the package:

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

---

### Step 2 — Build and install mcpelauncher-extract

`mcpelauncher-extract` is not distributed as a separate apt package — you compile it from source. The build typically completes in under a minute.

```sh
sudo apt install git cmake clang libzip-dev

git clone https://github.com/minecraft-linux/mcpelauncher-extract.git
cd mcpelauncher-extract
mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

---

### Step 3 — Back up the binaries

Both tools are GPL-licensed, meaning you are legally free to copy and redistribute them. **Back them up now**, before the project potentially disappears:

```sh
cp /usr/bin/mcpelauncher-client ~/mcpelauncher-client.bak
cp /usr/local/bin/mcpelauncher-extract ~/mcpelauncher-extract.bak
```

Store these somewhere safe — an external drive, cloud storage, or a private git repo.

---

### Step 4 — Get a Minecraft APK

Download the **x86_64** APK from [mcpelife.com](https://mcpelife.com).

> ⚠ **Architecture matters.** `mcpelauncher-client` only works with x86_64 APKs. ARM APKs — which are far more common — will not work. Double-check the architecture label before downloading.
>
> **On legality:** x86_64 APKs sourced from mcpelife.com are cracked builds and are not the official purchased version. No reliable method has been found to obtain an x86_64 APK through legal channels (i.e., from Google Play). If you find a way to do so legitimately, it should work with this method.

---

### Step 5 — Extract and play

Run these two commands. The extraction only needs to happen once per version:

```sh
# Extract game files from the APK (once per version)
mcpelauncher-extract ~/Downloads/minecraft-1.26.1.1-x86_64.apk \
  ~/.local/share/mcpelauncher/versions/1.26.1.1

# Launch the game
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/1.26.1.1
```

To install a different version, repeat this step with a different APK and a different destination folder name (matching the version number).

---

### Restoring from backup

If the upstream repository goes down and you need to reinstall the tools from your backups:

```sh
# Restore the binaries from backup
cp ~/mcpelauncher-client.bak /usr/local/bin/mcpelauncher-client
cp ~/mcpelauncher-extract.bak /usr/local/bin/mcpelauncher-extract

# Make sure they are executable
chmod +x /usr/local/bin/mcpelauncher-client
chmod +x /usr/local/bin/mcpelauncher-extract
```

Then use them exactly as in Step 5:

```sh
mcpelauncher-extract ~/Downloads/minecraft.apk ~/.local/share/mcpelauncher/versions/1.26.1.1
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/1.26.1.1
```

---

## Method 6 — GDK Version via ProtonGDK

Minecraft Bedrock has migrated from the UWP (Universal Windows Platform) format to the GDK (Game Development Kit) format on Windows. This method runs the actual Windows PC GDK build on Linux using ProtonGDK along with several proxy workarounds.

Unlike Methods 1–5, this does **not** involve Android or APKs — it runs the native Windows PC version of the game.

- Runs the real Windows PC build, not the Android port
- Setup is lengthy and complex — only recommended if Android-based methods are unavailable or unsuitable
- No single dedicated repository; relies on combining multiple tools and workarounds

**Links**

- [Video Tutorial](https://youtu.be/m76O2cRIEnM?si=YV2dxJpvLTYKtIDh)
- [Text-Based Tutorial](https://github.com/inbob1/mcbe-on-linux)

---

## Method 7 — Windows Virtual Machine

Run a full Windows installation inside a virtual machine (e.g., KVM/QEMU, VirtualBox, VMware) and play Minecraft Bedrock normally within it.

This is the most universal fallback — if Windows can run it, a Windows VM can run it. However, it comes with real trade-offs: GPU passthrough setup can be complex, there is inherent performance overhead from virtualization, and you need a valid Windows license.

**Use this as a last resort** when other methods have failed or are unavailable for your hardware configuration.

---

## Outdated Methods

These methods no longer work reliably and are documented here for historical reference only.

### Waydroid

Waydroid ran a full Android 11 container on Linux using kernel namespaces and allowed Minecraft to run as it would on a real Android device. This worked well for a time, but newer versions of Minecraft Bedrock get stuck at the loading screen and never progress. The method is effectively dead for current versions.

### CCMC Launcher (by Crow_Rei34 / JavierC)

Functioned similarly to Trinity Launcher — it extracted and ran custom APKs using the mcpelauncher toolchain. The repository has since been deleted or made private. Believed to have been discontinued when Trinity was introduced, as both share a developer (JavierC).

Historical video tutorials (Portuguese): [Tutorial 1](https://youtu.be/4K1X8PafWf0?si=jZe4X5xsA9nplT5h) | [Tutorial 2](https://youtu.be/VfVwqMcKhy0?si=_C70rhEyabASEKgb) | [Tutorial 3](https://youtu.be/sYvpEpwin6k?si=sD631OfATfYwzbH1)
