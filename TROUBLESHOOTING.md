# Troubleshooting

This page lists issues observed during the documented G1 Pro setup and explains what they do—and do not—establish.

## CMake cannot find Qt6Config.cmake

**Observed message:** CMake could not find the Qt 6 package configuration file while configuring the launcher.

**Context:** Qt runtime libraries were installed and the distributed launcher ran. The Qt 6 development SDK packages were not installed at the time of the check.

**Meaning:** Runtime presence and development package discovery are separate. CMake needs the Qt development package configuration files and components requested by the upstream CMakeLists.

**Next steps:**
1. Check the build instructions for the exact upstream revision.
2. Check whether `Qt6Config.cmake` exists on the system.
3. Install the matching Qt development packages only when required by current upstream instructions.
4. Re-run configuration and keep the complete CMake output.

## `qmake6` or `pkg-config Qt6Core` not found

These checks can fail even when the Qt runtime is installed. They concern development tools and metadata, not whether every Qt runtime library is missing.

Use the CMake error and the distribution package database to diagnose the development SDK separately.

## FileNotFoundError for `/caminho`

A setup command used `/caminho/jogo.iso` as a placeholder. Since that path did not exist, the script failed before testing an actual disc image.

Replace placeholder paths with the real path to legally obtained game data. Do not interpret this specific failure as an ISO compatibility result.

## Checking missing shared libraries

Run:

```bash
ldd path/to/program
```

On the documented host, all dependencies printed for `gow-session`, `gow-launcher` and `ps2EntryRunner` resolved. This result is specific to that machine and its installed libraries.

## Game ELF architecture differs from Linux host binaries

The inspected `SCUS_973.99` was a 32-bit MIPS-III ELF. The Linux `ps2EntryRunner` was an x86-64 Linux ELF.

This is expected in a recompilation/runtime arrangement: the game executable is input data for the runtime, not a program to execute directly as a native x86-64 Linux application.

## Steam shortcut does not launch

Check the following in order:

1. The script exists at the Steam Target path.
2. The script has execute permission.
3. All paths inside the script match the local checkout and game-data location.
4. The `.boot` file exists and points to the expected game ELF.
5. The runner and session helper exist and are executable.
6. Proton compatibility is disabled for the native Linux launch entry.
7. Run the script from a terminal to capture its error output.

## Bash reports syntax errors for CMake directives

CMake commands such as `find_package(...)` belong in CMake source files, not in a Bash terminal. To inspect a project, open its `CMakeLists.txt` rather than trying to execute CMake-language statements in the shell.

## Reporting a new issue

Include distribution and version, kernel and desktop session, GPU and driver, exact upstream revision/package, command executed and complete error output.

Redact usernames, personal paths, tokens, device identifiers, local network details and user data before publishing logs.
