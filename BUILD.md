# Build notes

This document records what happened during the local GOW I build investigation. It is explicit about failed and incomplete steps.

## Build environment observed

The CMake cache at `data/build-linux/gow1/CMakeCache.txt` recorded:

- Generator: Ninja
- Build type: Release
- C compiler: Clang
- C++ compiler: Clang++
- `BUILD_EXAMPLES=OFF`
- `BUILD_SHARED_LIBS=OFF`
- `BUILD_TESTING=ON`
- GLFW X11 enabled
- GLFW Wayland disabled

Clang 21.1.8 was reported by the setup process. The cache records compiler selection for this build directory; it does not establish that every component was successfully rebuilt.

## Launcher rebuild result

Running:

```bash
bash setup-linux.sh
```

reached the launcher configuration stage, where CMake detected Clang but failed because it could not find `Qt6Config.cmake`.

The distributed launcher binary remained available and its runtime dependencies resolved on this host. Therefore, the failed launcher rebuild does not mean the distributed launcher cannot run.

## ISO build attempt

A later command used the literal placeholder path `/caminho/jogo.iso`:

```bash
bash setup-linux.sh --discs "/caminho/jogo.iso" --jobs 2
```

It failed with `FileNotFoundError` for `/caminho`. This was a nonexistent placeholder path, not a valid test of an actual ISO and not evidence that a valid disc image is incompatible.

## Interpreting CMake settings correctly

- `BUILD_SHARED_LIBS=OFF` does not prove that every resulting executable is statically linked. The inspected Linux programs still had dynamic dependencies.
- `BUILD_TESTING=ON` means test targets/options were enabled in the cache; it does not mean tests were actually run or passed.
- The configured Clang/Ninja generator does not prove every target completed successfully.
- `-march=x86-64-v2` was observed in commands for some Rabbitizer C sources; it should not be generalized to the entire project.

## Current conclusion

The reliable result is that the prebuilt Linux runtime components used by this setup launched the game successfully on the documented G1 Pro. Rebuilding the Qt launcher was not completed because CMake could not locate the Qt 6 development package configuration.

## Before attempting a rebuild

1. Read the instructions for the exact upstream revision/package being used.
2. Confirm whether the upstream setup script is `setup-linux.sh`, `build.sh` or something else for that revision.
3. Install only development packages documented for that revision and distribution.
4. Use real paths to legally obtained game data; do not copy placeholder examples literally.
5. Preserve build logs and distinguish compile, link, test and runtime results.
