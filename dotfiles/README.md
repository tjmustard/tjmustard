# dotfiles

Shell configuration for this machine, tracked so it can be read, diffed and
carried. The files here are the originals; the home directory holds symlinks:

| Symlink | Points at |
| --- | --- |
| `~/.zshrc` | `dotfiles/zshrc` |
| `~/.zprofile` | `dotfiles/zprofile` |

Built from a portable zsh setup document (zsh 5.9 + starship, no framework, one
readable file), adapted for a machine where the Homebrew prefix belongs to an
admin account:

- Tools live in `~/.local/bin` (starship, zoxide, fzf, bat, gh); `eza` and
  `htop` come from Homebrew.
- Plugins are cloned under `~/.config/zsh/plugins/`.
- `compinit` runs with `-i`, because Homebrew's `site-functions` are admin-owned
  here and a plain `compinit` stops to ask about "insecure directories" on every
  new shell.
- The nvm block is load-bearing: `rig` and the OpenRig daemon resolve `node`
  from it. Do not drop it.

`~/.tmux.conf` is **not** tracked: OpenRig writes a managed block into it.

## Secrets

`~/.zshrc.local` is sourced when present and is gitignored. Machine-specific
settings and secrets go there, never in `zshrc`.

## Reload

```sh
usource        # alias for: source ~/.zshrc
```
