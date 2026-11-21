# Linux dependencies

This page records runtime and build dependencies observed during the G1 Pro setup. It is not a universal installation recipe. Package names vary between distributions and upstream may change its requirements.

## Runtime components observed

The distributed Linux programs were inspected with `file` and `ldd`.

| Program | Format / role | Observed result |
|---|---|---|
| `gow-session` | x86-64 Linux ELF; Qt-based session helper | Dynamic dependencies resolved on this host |
| `gow-launcher` | x86-64 Linux ELF; Qt Quick launcher | Dynamic dependencies resolved on this host |
| `ps2EntryRunner` | x86-64 Linux ELF; PS2 runtime runner | Dynamic dependencies resolved on this host |
| `SCUS_973.99` | MIPS-III 32-bit ELF game executable | Game data; not a native x86-64 Linux program |

The launcher dependency inspection included Qt 6 Core/Gui/QML/Quick/Quick Controls, SDL2, OpenGL/GLX/EGL, ALSA, PulseAudio, libsamplerate, libsndfile, X11, Wayland, xkbcommon, DRM/GBM, GLib, fontconfig, FreeType, HarfBuzz, PNG, ICU and DBus-related libraries.

The session helper dependency inspection included Qt 6 Core and associated system libraries. The runner inspection included the system C/C++ runtime, libm, OpenGL/GLX, X11 and XCB libraries.

These are observations from the local binaries. `ldd` includes transitive dependencies; its output is not a definitive list of direct link requirements.

## Development tools observed

The following tools were installed or detected during setup:

- CMake
- Ninja
- Clang / Clang++
- GCC / G++
- build-essential
- Python 3
- pkg-config

The GOW I build cache records Ninja as the CMake generator and Clang/Clang++ as the configured compilers. Clang 21.1.8 was reported during setup.

Some development packages were installed while working on more than one recompilation project on this machine. Their installation alone is not proof that the GOW I build requires them.

## Qt 6 development SDK

The upstream launcher CMake configuration requests Qt 6 components including Core, Gui, QML, Quick and Quick Controls 2. The local launcher rebuild attempt failed because CMake could not locate `Qt6Config.cmake`.

Qt runtime libraries were present and the distributed launcher executed. This distinguishes runtime libraries from the development SDK: having the runtime installed does not necessarily provide the CMake package configuration files and headers needed to compile the launcher.

The following development packages were not present at the time of the check:
- `qt6-base-dev`
- `qt6-declarative-dev`
- `qt6-tools-dev`

Do not install these blindly: first check current upstream build instructions and the Qt package names for your Ubuntu release.

## Useful inspection commands

```bash
file path/to/program
ldd path/to/program
```

Check unresolved libraries in the output. Do not assume every library printed by `ldd` is a direct dependency of the executable.

For CMake discovery problems:

```bash
find /usr /opt -name Qt6Config.cmake 2>/dev/null
```

Consult upstream documentation before adding repositories or installing third-party SDKs.
