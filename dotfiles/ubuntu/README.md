# dotfiles/ubuntu

The same portable zsh setup as the parent folder, adapted for Ubuntu 26.04
with the Ptyxis terminal. The files here are the originals; the home directory
holds symlinks:

| Symlink | Points at |
| --- | --- |
| `~/.zshrc` | `dotfiles/ubuntu/zshrc` |
| `~/.tmux.conf` | `dotfiles/ubuntu/tmux.conf` |
| `~/.local/share/org.gnome.Ptyxis/palettes/Portable.palette` | `dotfiles/ubuntu/ptyxis/Portable.palette` |

Differences from the macOS config:

- Tools come from `apt`, not Homebrew. `uv` is installed to `~/.local/bin` by
  its own installer. Homebrew is still picked up if present.
- zsh plugins are found under `/usr/share`, where the apt packages put them.
- Ubuntu ships `bat` as `batcat`; the aliases handle both names.
- `ls` gets `--color=auto` (GNU `ls` ignores `CLICOLOR`).
- No `zprofile`: Ubuntu needs no `brew shellenv`.

## Rebuild

```sh
sudo apt install -y zsh starship zoxide fzf eza bat \
  zsh-autosuggestions zsh-syntax-highlighting tmux gh htop
chsh -s "$(command -v zsh)"
curl -LsSf https://astral.sh/uv/install.sh | sh        # optional

# JetBrains Mono Nerd Font, per user
mkdir -p ~/.local/share/fonts/JetBrainsMonoNerd
curl -fsSL https://github.com/ryanoasis/nerd-fonts/releases/latest/download/JetBrainsMono.tar.xz \
  | tar -xJ -C ~/.local/share/fonts/JetBrainsMonoNerd --wildcards '*.ttf'
fc-cache -f

# Symlinks
ln -sf "$PWD/zshrc" ~/.zshrc
ln -sf "$PWD/tmux.conf" ~/.tmux.conf
mkdir -p ~/.local/share/org.gnome.Ptyxis/palettes
ln -sf "$PWD/ptyxis/Portable.palette" ~/.local/share/org.gnome.Ptyxis/palettes/

# Ptyxis font, palette, cursor
U=$(gsettings get org.gnome.Ptyxis default-profile-uuid | tr -d "'")
gsettings set org.gnome.Ptyxis use-system-font false
gsettings set org.gnome.Ptyxis font-name 'JetBrainsMono Nerd Font Mono 15'
gsettings set org.gnome.Ptyxis cursor-blink-mode 'on'
gsettings set "org.gnome.Ptyxis.Profile:/org/gnome/Ptyxis/Profiles/$U/" palette 'Portable'
```

## Secrets

`~/.zshrc.local` is sourced when present and is not tracked. Machine-specific
settings and secrets go there, never in `zshrc`.
