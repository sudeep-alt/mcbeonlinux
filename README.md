# Minecraft Bedrock on Linux

A complete guide to every known method of running Minecraft Bedrock Edition on Linux.

> [!IMPORTANT]
> All Android-based methods (Methods 1–5) ultimately depend on `mcpelauncher-client` and `mcpelauncher-extract` from the `mcpelauncher-manifest` project. If that project ever becomes unavailable, every Android-based launcher listed below will eventually stop working unless you already have the required binaries.

---

# Method 1: mcpelauncher

The original launcher and the foundation of nearly every Android-based Bedrock launcher on Linux. It requires owning Minecraft through Google Play and automatically downloads the APK using your Google account.

> [!WARNING]
> ChristopherHX (maintainer of mcpelauncher) [stated](https://discord.com/channels/429580677617418240/429580677617418243/1475181891043725404) on the [official Discord](https://discord.gg/UkXJMCcxhr) on **22 February 2026**:
>
> > "Yes we are sitting on top a cave full of tnt, you have no long term update warranties. Update delays / stops could happen at any point of time, since the DRM has been enabled."

### Features

- Legal; uses your own purchased copy of Minecraft.
- Downloads the APK directly from Google Play.
- Custom APKs are **not officially supported**.
- The first launcher ever to run Minecraft Bedrock on Linux.

### Resources

- [Repository](https://github.com/minecraft-linux/mcpelauncher-manifest)
- [Documentation](https://mcpelauncher.readthedocs.io/en/latest/getting_started/index.html)
- [Video Tutorial](https://youtu.be/rg_OH-nmoeQ?si=RNMzFO5ml4G2WJbF)

---

# Method 2: Trinity Launcher

> [!WARNING]
> The highest supported Minecraft version on **x86_64** systems with this launcher is **1.26.31**.

A modern launcher built on top of the standard mcpelauncher utilities. It adds version management together with integrated mod and texture pack support.

### Features

- Uses `mcpelauncher-client`
- Uses `mcpelauncher-extract`
- Supports custom APKs
- Built with C++
- Built-in version management

### Resources

- [Repository](https://github.com/Trinity-LA/Trinity-Launcher)
- [Documentation](https://trinitylauncher.vercel.app/)
- [Video Tutorial](https://youtu.be/ZMAamMBm8Go?si=qwZ7xOhtclxPyDw7)

---

# Method 3: Cianova Launcher

Developed by the *Las Tortuguitas de Ezku* community.

A Python launcher featuring a graphical interface built with CustomTkinter.

### Features

- Uses `mcpelauncher-client`
- Uses `mcpelauncher-extract`
- Supports custom APKs
- Built with Python

### Resources

- [Repository & Documentation](https://github.com/PlaGaPlusDev/CianovaLauncher-mcpelauncher)
- [Video Tutorial](https://youtu.be/-tl4ZSJ3DSE?si=-xfBd_xV1Of5XfHI)

---

# Method 4: Iosel Edition

A modified AppImage fork of mcpelauncher.

It claims to remove the dependency on the Flatpak runtime and was created by a contributor associated with the Trinity project.

### Features

- Supports custom APKs
- Standalone AppImage
- No Flatpak runtime required

### Resources

- [Repository & Documentation](https://github.com/IoselDev/Minecraft-Bedrock-Linux-Iosel-Edition)
- [Video Tutorial](https://youtu.be/BAZqN_NhIj4?si=eEuOY4TvDeE_VpWQ)

---

# Method 5: Raw CLI (Recommended)

Instead of using a launcher, you can directly use the underlying mcpelauncher utilities from the terminal.

Every launcher above ultimately performs these same commands internally.

---

## Step 1 — Install `mcpelauncher-manifest`

```sh
curl -fsSL https://minecraft-linux.github.io/pkg/deb/pubkey.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/mcpelauncher.gpg > /dev/null

echo "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mcpelauncher.gpg] https://minecraft-linux.github.io/pkg/deb noble main" | sudo tee /etc/apt/sources.list.d/mcpelauncher.list

sudo apt update

sudo apt install mcpelauncher-manifest
```

---

## Step 2 — Build and install `mcpelauncher-extract`

`mcpelauncher-extract` is not distributed as an independent package, so it must be compiled manually.

```sh
sudo apt install git cmake clang libzip-dev

git clone https://github.com/minecraft-linux/mcpelauncher-extract.git

cd mcpelauncher-extract

mkdir build

cd build

cmake ..

make -j$(nproc)

sudo make install
```

---

## Step 3 — Obtain an x86_64 APK

Download an **x86_64** Minecraft Bedrock APK from **[MCPELife](https://mcpelife.com)**.

> [!IMPORTANT]
> Download the **x86_64** APK, **not** the ARM version.

---

## Step 4 — Extract the APK

The APK must first be extracted before it can be launched.

### Command syntax

```sh
mcpelauncher-extract {PATH_TO_APK} {STORE_PATH}
```

### Arguments

- `{PATH_TO_APK}` — Path to the downloaded x86_64 APK.
- `{STORE_PATH}` — Directory where the extracted version will be stored.

### Example

```sh
mcpelauncher-extract ~/Downloads/minecraft.apk ~/Downloads/mcpelauncher_versions/1.21.0
```

> [!TIP]
> You may also store extracted versions inside:
>
> `~/.local/share/mcpelauncher/versions`
>
> This directory is automatically created after installing `mcpelauncher-manifest`.
>
> Example:
>
> ```sh
> mcpelauncher-extract ~/Downloads/minecraft.apk ~/.local/share/mcpelauncher/versions/1.21.0
> ```

---

## Step 5 — Launch

### Command syntax

```sh
mcpelauncher-client -dg {STORED_PATH}
```

### Arguments

- `{STORED_PATH}` — The directory created during extraction.

### Example

```sh
mcpelauncher-client -dg ~/Downloads/mcpelauncher_versions/1.21.0
```

or

```sh
mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/1.21.0
```

---

## Adding a new version

To add another version, simply repeat **Steps 3–5** using a different APK and destination folder.

---

## Installing mods, resource packs, worlds, and behavior packs

All user data managed by `mcpelauncher-client` is stored inside:

```text
~/.local/share/mcpelauncher
```

This directory is automatically created after installing `mcpelauncher-manifest`.

To open it in your file manager:

```sh
cd ~/.local/share/mcpelauncher/games/com.mojang

xdg-open .
```

Place content inside the appropriate folders.

Examples:

- Resource packs → `resource_packs`
- Behavior packs → `behavior_packs`
- Worlds → `minecraftWorlds`

> [!NOTE]
> Everything stored inside `~/.local/share/mcpelauncher/` is shared globally across every version launched through `mcpelauncher-client`.

---

# Obtaining x86_64 APKs

`mcpelauncher-client` only works with **x86_64** Minecraft APKs.

## Cracked APKs

Cracked x86_64 APKs are available from **[MCPELife](https://mcpelife.com)**.

Other websites may also provide them, but this is one known source.

---

## Official APKs (Legal)

### Android Studio Emulator

This method is currently **untested**.

It may be possible to create an **x86_64** Android emulator using **[Android Studio](https://developer.android.com/studio)** and have Google Play provide the x86_64 APK.

Again, this has not been verified.

---

### Google Play API

According to the creator of `mcpelauncher` and several members of the project's Discord server, the launcher retrieves x86_64 APKs through a Google Play API.

They also claim the approach is similar to **[Aurora Store](https://auroraoss.com/)**.

The implementation has not been publicly documented, but it is worth mentioning.

---

# Backing up the binaries

Keeping local copies of the launcher binaries ensures they remain usable even if the repositories disappear.

```sh
mkdir ~/mcpelauncher-backup

cp /usr/bin/mcpelauncher-client ~/mcpelauncher-backup/

cp /usr/local/bin/mcpelauncher-extract ~/mcpelauncher-backup/
```

> [!NOTE]
> A **[backup of the binaries](https://github.com/MeantPlace279/mcbeonlinux/blob/main/mcpelauncher-backup.zip)** is also available in this repository for convenience.

---

# Restoring from backup

If the repositories become unavailable, place the backed-up binaries inside:

```text
~/mcpelauncher-backup
```

Then run:

```sh
cp ~/mcpelauncher-backup/mcpelauncher-extract /usr/local/bin/

cp ~/mcpelauncher-backup/mcpelauncher-client /usr/bin/

chmod +x /usr/local/bin/mcpelauncher-extract

chmod +x /usr/bin/mcpelauncher-client
```

After restoring, use them exactly as before:

```sh
mcpelauncher-extract ~/Downloads/minecraft.apk ~/.local/share/mcpelauncher/versions/{VERSION}

mcpelauncher-client -dg ~/.local/share/mcpelauncher/versions/{VERSION}
```

---

# Method 6: GDK Version via ProtonGDK

Minecraft Bedrock on Windows has transitioned from UWP to GDK.

This method runs the native Windows GDK version through ProtonGDK together with proxy workarounds.

### Features

- Runs the actual Windows PC version.
- Does not rely on Android.
- Lengthy setup.
- Recommended only if Android-based methods fail.

### Resources

- [Video Tutorial](https://youtu.be/m76O2cRIEnM?si=YV2dxJpvLTYKtIDh)
- [Text Tutorial](https://github.com/inbob1/mcbe-on-linux)

---

# Method 7: Windows Virtual Machine

Run Windows inside a virtual machine and play Minecraft Bedrock normally.

### Features

- Native Windows version
- Works reliably
- Higher resource usage
- Best used as a last resort

---

# Outdated Methods

## Waydroid

Waydroid previously ran Minecraft inside a complete Android environment.

Recent Minecraft versions become stuck at the loading screen, making this method unusable.

---

## CCMC Launcher by Crow_Rei34

CCMC Launcher worked similarly to Trinity by extracting and launching custom APKs.

The repository has since been deleted or made private.

It is believed to have been discontinued when Trinity Launcher was introduced, as both projects shared a developer (JavierC).

### Historical Tutorials (Portuguese)

1. [Tutorial 1](https://youtu.be/4K1X8PafWf0?si=jZe4X5xsA9nplT5h)
2. [Tutorial 2](https://youtu.be/VfVwqMcKhy0?si=_C70rhEyabASEKgb)
3. [Tutorial 3](https://youtu.be/sYvpEpwin6k?si=sD631OfATfYwzbH1)
