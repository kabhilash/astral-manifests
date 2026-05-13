---
name: astraos-build-recipe
description: Bitbake a single AstraOS recipe (optionally a specific BitBake task on it) for a target MACHINE. Use whenever the user asks to "bitbake recipe X for Y", "build just qtbase for cm5", "rerun -c populate_sysroot on libusb1 for raspberrypi5", "build only the astrax-bt recipe", "compile spdlog by itself", or any phrasing that means "run BitBake on a single recipe rather than a whole image". For full image builds use `astraos-build-image` or `astraos-build-prod`. For arbitrary BitBake invocations with multiple flags use `astraos-bitbake`.
---

Run the recipe subcommand of the AstraOS build script:

```
cd /Users/akothapalli/Projects/AstraA/X/AstraOS
./scripts/build recipe <MACHINE> <RECIPE> [TASK]
```

The optional `[TASK]` argument is a BitBake task like `cleansstate`,
`populate_sysroot`, `compile`, `do_unpack`, etc. If the user doesn't
specify a task, omit it — BitBake will run the default task chain
(`do_build`).

## Extracting parameters from the user's request

**MACHINE** — same mapping as `astraos-build-image`. If unspecified,
ask the user which target.

**RECIPE** — the package name as it appears in BitBake. Common ones the
user might mention:

| User says | RECIPE |
|---|---|
| "qtbase", "Qt base" | `qtbase` |
| "Qt declarative", "qml engine" | `qtdeclarative` |
| "spdlog" | `spdlog` |
| "libusb" | `libusb1` |
| "libgpiod" | `libgpiod` |
| "the AstraX app", "astrax-bt", "the benchtop app" | `astrax-bt` |
| "nexus" | `nexus` |
| "RAUC" | `rauc` |
| "the kernel", "linux", "linux kernel" | `virtual/kernel` (or the BSP-specific one) |
| "u-boot", "bootloader" | `virtual/bootloader` (or the BSP-specific one) |

**TASK** — only if the user says "rerun X" or "do task Y". Common ones:

- `cleansstate` — wipe sstate cache for this recipe
- `clean` — remove build artefacts but keep sstate
- `populate_sysroot` — make headers/libs available to dependent recipes
- `compile` — just the build step
- `unpack` / `patch` / `configure` — earlier stages
- `package` — produce the binary package
- `devshell` — drop into a shell with the recipe's build env

## Examples

User says "rebuild qtbase for raspberrypi5":
```
./scripts/build recipe raspberrypi5 qtbase
```

User says "wipe sstate for libusb on cm5":
```
./scripts/build recipe raspberrypi-cm5-io-board libusb1 cleansstate
```

User says "drop me into the qtbase devshell on the variscite":
```
./scripts/build recipe imx8mp-var-dart qtbase devshell
```

If the user wants a more complex BitBake invocation (multiple recipes,
flags like `-k -v`, environment overrides), use `astraos-bitbake`
instead.
