---
name: astraos-shell
description: Open an interactive bash shell inside the AstraOS build environment (devcontainer) with `setup-environment` already sourced for the given MACHINE. Use whenever the user wants to "open an AstraOS shell", "give me a build shell for raspberrypi5", "drop me into the devcontainer for cm5", "I want to poke around BitBake interactively for the variscite", "let me run multiple commands in the build env without re-sourcing each time", or any phrasing that means "I need a long-lived terminal session in the AstraOS build env". For one-off invocations, prefer the more specific skills (`astraos-bitbake`, `astraos-build-recipe`) instead — they don't require leaving an interactive shell open.
---

Run the shell subcommand of the AstraOS build script:

```
cd /Users/akothapalli/Projects/AstraA/X/AstraOS
./scripts/build shell <MACHINE>
```

This is **interactive**: the user lands at a bash prompt inside the
devcontainer (running on the build machine via SSH by default). The
prompt has BitBake on PATH, `setup-environment <MACHINE>` already
sourced, and `cwd = /home/akothapalli/yocto/astraos/build-<MACHINE>/`.
The user types `bitbake ...` directly, edits files, runs
`devtool modify` workflows, etc.
Exiting with `Ctrl-D` or `exit` returns control to the user's host
shell.

## Extracting MACHINE

Same as `astraos-build-image`. If unspecified, ask which target — the
shell's `bblayers.conf` and `local.conf` are MACHINE-dependent, so
this matters.

## When to use vs. alternatives

- One-off bitbake call → `astraos-bitbake` (no need for a shell)
- One-off recipe build → `astraos-build-recipe`
- Full image → `astraos-build-image` / `astraos-build-prod`
- Multiple commands in a row, exploration, devtool workflows,
  troubleshooting → **this skill**

## Practical note

Because the shell runs over SSH from the user's local Mac to the
build machine, the user must keep the SSH session alive. If they
expect a long-running interactive session (e.g., devtool modify +
edit + build + deploy cycles), suggest `tmux` / `screen` on the build
machine, or just keep their terminal open.
