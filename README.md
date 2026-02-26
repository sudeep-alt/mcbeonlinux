# Minecraft Bedrock on Linux

A complete guide to every known method.

**Important:** All Android-based methods (1–5) depend on `mcpelauncher-client` and `mcpelauncher-extract` from the mcpelauncher-manifest project. If that project dies, they all die — unless you have the binaries backed up.

---

## Method 1 — mcpelauncher

The original. Has been the go-to for years. Requires owning Minecraft on Google Play — downloads the APK automatically through your account.

> ⚠ The maintainer (ChristopherHX) stated on the official Discord:
> *"Yes we are sitting on top a cave full of tnt, you have no long term update warranties. Update delays / stops could happen at any point of time, since the DRM has been enabled."*

- Legal: Yes — uses your own purchased copy
- APK source: Downloads automatically via Google Play login
- Custom APKs: Not officially supported
- First ever launcher to run Bedrock on Linux

[Repo](https://github.com/minecraft-linux/mcpelauncher-manifest) | [Docs](https://mcpelauncher.readthedocs.io/en/latest/getting_started/index.html) | [Tutorial](https://youtu.be/rg_OH-nmoeQ?si=RNMzFO5ml4G2WJbF)

---

## Method 2 — Trinity Launcher

Well-maintained and formal. Has its own mod/texture pack management on top of the standard mcpelauncher utilities.

- Uses `mcpelauncher-client` and `mcpelauncher-extract`
- Runs custom APKs
- Version management, export/import, shortcuts, Discord Rich Presence
- Built with C++ and Qt6


[Repo](https://github.com/Trinity-LA/Trinity-Launcher) | [Docs](https://trinitylauncher.vercel.app/) | [Tutorial](https://youtu.be/ZMAamMBm8Go?si=qwZ7xOhtclxPyDw7)

---

## Method 3 — Cianova Launcher

Made by a user in the Trinity Launcher Discord. Python-based GUI using customtkinter.

- Uses `mcpelauncher-client` and `mcpelauncher-extract`
- Runs custom APKs
- Built with Python — easier to read and modify than Trinity
- Supports Nvidia Prime, GameMode, Zink, dependency checker

[Repo + Docs](https://github.com/PlaGaPlusDev/CianovaLauncher-mcpelauncher) | [Tutorial](https://youtu.be/-tl4ZSJ3DSE?si=-xfBd_xV1Of5XfHI)

---

## Method 4 — Iosel Edition

A modified AppImage fork of mcpelauncher. Claims to not depend on Flatpak runtime. Made by a contributor associated with the Trinity project.

- Can configure custom APKs
- Standalone — no Flatpak runtime required

[Repo + Docs](https://github.com/IoselDev/Minecraft-Bedrock-Linux-Iosel-Edition) | [Tutorial](https://youtu.be/BAZqN_NhIj4?si=eEuOY4TvDeE_VpWQ)

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
cp /usr/bin/mcpelauncher-client ~/mcpelauncher-client
cp /usr/local/bin/mcpelauncher-extract ~/mcpelauncher-extract
```

### Step 4 — Get a Minecraft APK

Download the **x86_64** APK from [mcpelife.com](https://mcpelife.com). Make sure it's x86_64, not ARM.

### Step 5 — Extract and play

```sh
# extract (once per version)
mcpelauncher-extract ~/Downloads/minecraft-x86_64.apk \
  ~/.local/share/mcpelauncher/versions/{VESRION}

# play
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/{VERSION}
```

To install a different version, repeat Step 5 with a different APK and version folder name.

### NOTE
`mcpelauncher-client` only works with x86_64 APKs. Such APKs are very hard to find. Cracked x86_64 APKs are available at [mcpelife.com](https://mcpelife.com). But I couldn't find any reliable way to get x86_64 APK for the legal version of MCPE. If you manage to do so, that should still work.

### Restoring from backup
If the repo goes down and you need to restore the binaries you backed up:
```bash
cp ~/mcpelauncher-extract /usr/local/bin/mcpelauncher-extract 
cp ~/mcpelauncher-client /usr/bin/mcpelauncher-client 
chmod +x /usr/bin/mcpelauncher-client 
chmod +x /usr/local/binmcpelauncher-extract 
```

Then use them exactly as normal:
```bash
mcpelauncher-extract ~/Downloads/minecraft.apk ~/.local/share/mcpelauncher/versions/{VERSION}
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/{VERSION}
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
