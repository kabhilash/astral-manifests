---
name: astraos-build-container
description: Rebuild the AstraOS Yocto build devcontainer Docker image. Use whenever the user asks to "rebuild the devcontainer", "rebuild the AstraOS container", "rebuild the docker image", "refresh the build container", or whenever they've changed `.devcontainer/Dockerfile` or `devcontainer.json` and need to apply those changes. By default the container build runs on the AstraOS build machine via SSH; pass `--local` if the user wants it to run on their laptop's Docker daemon instead.
---

Run the container subcommand of the AstraOS build script:

```
cd /Users/akothapalli/Projects/AstraA/X/AstraOS
./scripts/build container
```

If the user explicitly says "build it locally" or similar, prepend `--local`:

```
./scripts/build --local container
```

The script handles the SSH dispatch to the build machine
(`akothapalli@10.11.12.20`, workspace `/yocto/astraos`) on its own when
running in remote mode. Just invoke the command — the script knows what
to do.

The container image is tagged `astraos-builder:latest`. After it
finishes, subsequent `./scripts/build image|sdk|recipe|...` calls will
use the new image automatically.
