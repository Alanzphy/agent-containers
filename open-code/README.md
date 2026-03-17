# Open Code

## Build Instructions

Use the top level `Makefile` to build this. It injects things in to the build
so the container works correctly for your user under `podman`.

```bash
make open-code
```

You can add more local tools to the container to be installed via `apt-get` by
extending the `LOCAL_TOOLS` list in the top-level `Makefile`.

## Run Instructions

The repository now includes `open-code/opencode-project`, a launcher script that:

- Detects `podman` or `docker` automatically.
- Uses a container name based on project name + path hash.
- Mounts your current repository into `/app`.
- Reuses your global OpenCode config/share/state by default (so connected models/tokens persist).
- Supports full per-project isolation with `--all-isolate` (or `OPENCODEBOX_ISOLATE=1`).

Install it once:

```bash
install -m 0755 ./open-code/opencode-project ~/.local/bin/opencode-project
```

Then add an alias in your shell (`~/.zshrc` or `~/.bashrc`):

```bash
alias opencodebox='opencode-project'
alias opencodebox_isolate='opencode-project --all-isolate'
```

Usage from any project directory:

```bash
opencodebox
```

Debug the generated container command without launching it:

```bash
opencodebox --dry-run
```

Project isolation is stored under:

- `${XDG_DATA_HOME:-$HOME/.local/share}/opencode-projects/<project-id>/state`
- `${XDG_DATA_HOME:-$HOME/.local/share}/opencode-projects/<project-id>/share`
- `${XDG_CONFIG_HOME:-$HOME/.config}/opencode-projects/<project-id>`

This keeps each project's OpenCode session data separate while keeping one simple alias.

If you want full isolation for one run:

```bash
opencodebox --all-isolate
```

Equivalent environment-variable mode:

```bash
OPENCODEBOX_ISOLATE=1 opencodebox
```

## References

* [Documentation](https://opencode.ai/docs)
* [Github Repo](https://github.com/sst/opencode)
