# dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io/).

## How it works

**chezmoi** is the dotfile manager driving everything here. It handles templating, symlinking config files into the right places (`dot_` prefix → `~/.`), and running lifecycle scripts.

### Bootstrap

Run `setup` on a fresh machine:

```bash
bash setup
```

This checks if `chezmoi` is installed and, if not, bootstraps it directly from the repo:

```sh
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply git@github.com:NautalisCat/dotfiles.git
```

If chezmoi is already present, the script exits cleanly (idempotent).

### Tool management (mise)

[mise](https://mise.jdx.dev/) is used as the runtime/tool version manager (think `asdf` or `nvm`, but faster and unified).

- **`.chezmoiexternals/mise.toml`** — tells chezmoi to download the `mise` binary itself directly from `mise.jdx.dev`, targeting your OS and architecture automatically via template variables.
- **`dot_config/mise/config.toml`** — declares the tools mise should install:
  - `bat` (a `cat` replacement with syntax highlighting)
  - `chezmoi` (so mise manages chezmoi after the initial bootstrap)
- **`.chezmoiscripts/run_onchange_after_install_packages.sh.tmpl`** — a chezmoi script that re-runs `mise install` whenever `mise/config.toml` changes (tracked via its SHA256 hash in the script header).

## File map

| Repo file | Deployed to |
|---|---|
| `dot_bashrc` | `~/.bashrc` |
| `dot_vimrc` | `~/.vimrc` |
| `dot_tmux.conf` | `~/.tmux.conf` |
| `dot_config/mise/config.toml` | `~/.config/mise/config.toml` |
| `.chezmoiexternals/mise.toml` | downloads `mise` binary to `~/.local/bin/mise` |
| `setup` | run manually; excluded from chezmoi deployment (`.chezmoiignore`) |
