# Configuration and Steam launch

This page documents the working local launch arrangement used for God of War I on the G1 Pro. Paths are specific to that machine and must be adapted elsewhere.

## Local layout

Upstream checkout:

```text
<PROJECT_DIRECTORY>/gow-native-pc
```

Steam launch script:

```text
<USER_HOME>/bin/gow-native-direct
```

The exact home directory and mount path are intentionally omitted from this public guide. Steam was installed from the distribution package.

## Steam shortcut

The non-Steam shortcut was configured with:

- Target: the local `gow-native-direct` script
- Start In: blank
- Launch Options: blank
- Steam compatibility / Proton: disabled

The game was launched through the native Linux helper rather than through Proton.

## What the launch script does

The local script:

1. Defines the repository, boot file, runner, game ELF and session helper paths.
2. Checks that required files exist and that the runner/helper are executable.
3. Reads the generated `.boot` file and validates its header and ELF path.
4. Exports recognized environment variables with prefixes `GOW_`, `PS2X_` and `TOBIICHI_`.
5. Optionally exports the save-slot assets directory when its marker file exists.
6. Creates a per-session directory and JSON metadata file.
7. Invokes `gow-session` with the session directory, runner, game ELF and repository path.

This script is a local integration wrapper, not an upstream installer. Its absolute paths and assumptions must be adapted before reuse.

## Configuration values observed

The generated boot configuration included these values:

| Setting | Observed value |
|---|---|
| Resolution | 1280 × 800 |
| Aspect ratio | 16:9 automatic |
| Fullscreen | Enabled |
| VSync | Enabled |
| FXAA | Enabled |
| Sharpening | 0.20 |
| Anisotropic filtering | 8× |
| Renderer | Hardware |
| Scaler | Sharp |
| Language | Portuguese (`pt`) |
| Audio device | `default` |
| Audio volume | 100 |
| PS2 hardware scale | 2 |

These are values from one working setup, not recommended defaults for every display or GPU.

The configuration also contained `TOBIICHI_VENTANA_FPS=60`. The meaning of this option was not independently established; it should not be described as a confirmed frame-rate cap.

## Controller mapping

A custom controller mapping was configured in the boot file using the project's mapping syntax. The values observed included:

```text
_custom=1;square=8;triangle=5;circle=6;cross=7;l1=9;r1=11;r2=10;l2=12;up=1;right=2;down=3;left=4;start=15;select=13;l3=16;r3=17
```

This reflects one controller setup and should be adapted to the controller and input backend in use. Do not assume button indexes are identical across devices.

## Saves and user data

The local setup referenced a save directory and save-slot assets under the game project data. Exact paths and save contents are intentionally omitted.

Save paths can contain personal data. Do not commit saves, save-slot contents, local configuration exports or other user-specific files to this documentation repository.

## Privacy

Before sharing logs or configuration files, remove usernames, absolute home-directory paths, machine identifiers, local network addresses, tokens and personal save data.
