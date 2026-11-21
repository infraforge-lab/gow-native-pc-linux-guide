# God of War Native PC port on Linux

A practical community guide to installing and running the **God of War (PS2) native PC port / recompilation project** on Linux. This is an independent guide, not an official Sony release and not an official document from the upstream maintainers.

> **You must provide your own legally obtained copy of the game.** The upstream project and this guide do not distribute the game, ISO images, BIOS files, extracted assets, saves or other proprietary game data.

> **Scope of this guide:** the installation flow below was tested only on
> Ubuntu 26.04.1 LTS (GNOME 50.1 / Wayland) with a Minisforum G1 Pro and
> NVIDIA RTX 5060 8 GB. Results on SteamOS, other Ubuntu releases,
> distributions, GPUs and desktop environments are unverified.

## Quick start

The Linux support is experimental. The prebuilt Linux package described by upstream targets Ubuntu 26.04 on x86-64; do not assume it works unchanged on every distribution.

### 1. Check the requirements

- x86-64 Linux system with an appropriate GPU driver
- A GPU and graphics driver compatible with the upstream runtime; check the current upstream Linux notes for the graphics capabilities required by your release
- Sufficient RAM for the launcher and compilation workload; memory use varies by system and build. The upstream documentation does not establish 16 GB as a universal requirement
- Free disk space for the launcher, runtime, prepared game data and generated build
- Your own compatible God of War disc image
- Internet access for downloading the runtime and build components

For exact package requirements and current limitations, read the upstream [Linux installation notes](https://github.com/Vnnc1/gow-native-pc/blob/main/docs/linux.md).

### 2. Download the Linux package

1. Open the upstream [Releases page](https://github.com/Vnnc1/gow-native-pc/releases).
2. Download the Linux x86-64 Ubuntu 26.04 experimental `.run` installer and the release's `SHA256SUMS.txt` file.
3. Keep both files in the same directory.

Verify the installer against its entry in `SHA256SUMS.txt`. Set `INSTALLER` to the exact name of the downloaded `.run` file:

```bash
INSTALLER='gow-native-pc-linux-x86_64-ubuntu26.04-experimental.run'

grep -F "$INSTALLER" SHA256SUMS.txt | sha256sum --check -
```

The release manifest uses the format `hash *filename`. The command selects the matching entry and passes it through standard input to `sha256sum --check -`, which verifies the downloaded installer. A successful check reports `OK`. If the command finds no matching entry or reports a failure, do not run the installer. Confirm that you downloaded the manifest from the same release as the installer and that the asset name matches exactly.

> Release formats can change. Follow the checksum filename and verification instructions attached to the specific release you download. For releases publishing a single `SHA256SUMS.txt`, use the command above; older releases may provide individual `.sha256` files.

### 3. Install the launcher

Run the downloaded installer from its directory:

```bash
bash gow-native-pc-linux-x86_64-ubuntu26.04-experimental.run
```

The upstream Linux installer installs under the user's local share directory by default and does not require root. It does not install system dependencies. It also refuses to overwrite an existing destination to protect existing game data and saves.

If the release uses a different asset name, substitute the exact downloaded filename.

### 4. Open the launcher and prepare your own game image

The installer prints or documents its installation destination. Open the installed directory and start the launcher:

```bash
cd "$HOME/.local/share/gow-native-pc-<version>"
bash start-linux.sh
```

Replace `<version>` with the version directory actually installed.

In the launcher's **Install** page:

1. Install any missing system dependencies requested by the launcher or upstream Linux guide.
2. Select **Add ISO / Install disc** (wording can vary by release) and choose your own compatible game image.
3. Follow the launcher steps to prepare the disc, fetch the required runtime and compile the game.
4. Wait for the process to finish; compilation time depends on CPU, memory, storage and downloads.

The ISO is processed locally. The project does not provide the game image. Do not use a disc image unless you have the legal right to use it.

### 5. Launch the game

When installation and compilation finish, use the launcher's **Play** action to confirm that the game starts correctly before configuring Steam.

### 6. Optional: add the working launch script to Steam

The Steam setup below documents the local integration used on the test machine in this guide. It is **not** a universal upstream Steam installer or a script automatically created by every Linux package.

1. Add a **Non-Steam Game** in Steam.
2. Set **Target** to the full path of your own executable launch wrapper. In the tested setup, the wrapper was stored at:

   ```text
   <HOME_DIRECTORY>/bin/gow-native-direct
   ```

3. Leave **Start In** and **Launch Options** blank, unless your own wrapper requires otherwise.
4. In the shortcut's **Compatibility** settings, leave Proton compatibility disabled for this native Linux launch.
5. Launch the shortcut and verify that the game opens.

The wrapper must be created and configured for your own checkout and installation paths. See [Configuration and Steam launch](CONFIGURATION.md) for what the tested wrapper validates and how it invokes the native session helper.

---

## Tested environment and results

| Distribution / hardware | What was tested | Result |
|---|---|---|
| Ubuntu 26.04.1 LTS, GNOME 50.1 Wayland, Minisforum G1 Pro, NVIDIA RTX 5060 8 GB | Native Linux runtime components and a local Steam launch wrapper | **Game launch confirmed** |
| Other Ubuntu releases | Not tested in this guide | Unknown |
| SteamOS / Steam Deck | Not tested in this guide | Not validated |
| Other distributions and GPUs | Not tested in this guide | Unknown |

The successful result above applies to the documented machine and software snapshot; it is not a compatibility guarantee for other systems.

### Test machine details

- **Device:** Minisforum G1 Pro
- **OS:** Ubuntu 26.04.1 LTS (Resolute)
- **Desktop:** GNOME 50.1 / Wayland
- **GPU:** NVIDIA GeForce RTX 5060 (8 GB)
- **Architecture:** x86_64
- **Storage:** NVMe SSD

This guide does not claim that every Linux distribution, GPU or Steam Deck configuration has been tested.

## Build from source (alternative)

If you are not using the prebuilt Ubuntu experimental package, follow the upstream instructions for your distribution and the exact revision you checked out.

The current upstream Linux development guide describes the source build entry point as:

```bash
bash build.sh
```

For a build that imports your own disc image, the upstream guide documents:

```bash
bash build.sh --discs "/path/to/your/game.iso" --jobs 2
```

Use the current upstream [Linux guide](https://github.com/Vnnc1/gow-native-pc/blob/main/docs/linux.md) for the complete dependency list and distribution-specific instructions. Do not assume that the older `setup-linux.sh` workflow described in historical investigation notes is the correct entry point for a newer upstream revision.

## Troubleshooting and technical notes

- [Dependencies](DEPENDENCIES.md) — runtime libraries, development tools and Qt SDK notes
- [Build notes](BUILD.md) — observed CMake settings and build investigation
- [Configuration and Steam launch](CONFIGURATION.md) — tested local wrapper and game settings
- [Troubleshooting](TROUBLESHOOTING.md) — issues and diagnostic steps
- [Credits and licensing](CREDITS.md) — upstream attribution and license notes
- [Documentation license](LICENSE) — CC BY 4.0 applies to the original documentation in this repository

## Important technical distinction

The game's `SCUS_973.99` executable is a 32-bit MIPS game ELF. It is not a native x86-64 Linux application. The host-side launcher and runtime tools are x86-64 ELF binaries.

## Project information

- **Upstream project:** [Vnnc1/gow-native-pc](https://github.com/Vnnc1/gow-native-pc)
- **Upstream snapshot examined during the original investigation:** 0.6.1
- **Upstream commit examined:** `4152d910a67564436db94f4c1b23f7ec4afce9db`

Upstream versions and Linux packaging can change. Always check the release notes and Linux documentation for the version you download.

## Independence and legal notice

This guide was written independently by a community user and is **not produced, reviewed, maintained or endorsed by the upstream project maintainers, Sony or the rights holders**. “God of War” is a trademark of its respective owner.

No game files, ISO images, BIOS files, proprietary assets, saves or compiled game binaries are distributed in this repository. You are responsible for obtaining and using your own game data lawfully and for complying with the upstream project's license.

See [CREDITS.md](CREDITS.md) for attribution and upstream licensing notes.
