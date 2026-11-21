# God of War Native PC on Linux

Independent documentation for running **God of War (2005)** through the native PC recompilation project on Linux.

This guide is based on a working installation on a Minisforum G1 Pro.

## Tested environment

- **Device:** Minisforum G1 Pro
- **Operating system:** Ubuntu 26.04.1 LTS (Resolute)
- **Desktop:** GNOME 50.1 / Wayland
- **GPU:** NVIDIA GeForce RTX 5060 (8 GB)
- **Architecture:** x86_64
- **Storage:** NVMe SSD

Versions of the NVIDIA driver and CUDA are environment-specific and may differ from your installation.

## Project

- Upstream project: https://github.com/Vnnc1/gow-native-pc
- Upstream snapshot examined: 0.6.1
- Upstream commit examined: `4152d910a67564436db94f4c1b23f7ec4afce9db`

This repository is an independent Linux usage and troubleshooting guide. It is not affiliated with or endorsed by the upstream project.

## Status

God of War I was launched successfully through Steam using the project's native Linux runtime components and a local launch script. Proton compatibility was disabled for this setup.

The upstream package manifest examined during documentation indicated `contains_game_data: false` and `steam_deck_validated: false`.

## Documentation

- [Dependencies](DEPENDENCIES.md)
- [Build notes](BUILD.md)
- [Configuration and Steam launch](CONFIGURATION.md)
- [Troubleshooting](TROUBLESHOOTING.md)
- [Credits and licensing](CREDITS.md)

## Important technical note

The game executable `SCUS_973.99` is a 32-bit MIPS game ELF. It is not a native x86_64 Linux executable. The host-side launcher and runtime tools are x86-64 ELF binaries.

## Legal notice

This repository does not distribute game files, ISO images, BIOS files, save data, proprietary assets, or compiled game binaries. You must provide your own legally obtained game data and comply with applicable laws and the upstream project's license.

See [CREDITS.md](CREDITS.md) for attribution and licensing notes.
