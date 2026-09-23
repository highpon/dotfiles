# dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io/).

## Setup

### 1. Install chezmoi

```sh
# Arch Linux / CachyOS
sudo pacman -S chezmoi

# Other platforms
sh -c "$(curl -fsLS get.chezmoi.io)"
```

### 2. Apply the dotfiles

On a new machine, clone and apply in one step
(chezmoi puts its copy of the repo in `~/.local/share/chezmoi`):

```sh
chezmoi init --apply git@github.com:highpon/dotfiles.git
```

If you already cloned this repository somewhere else (e.g. `~/git/dotfiles`),
point chezmoi at that clone instead:

```sh
mkdir -p ~/.config/chezmoi
echo 'sourceDir = "~/git/dotfiles"' > ~/.config/chezmoi/chezmoi.toml
```

Then preview the changes and apply them:

```sh
chezmoi diff   # show what will change in $HOME
chezmoi apply  # write the files
```

Existing files such as `~/.zshrc` will be overwritten, so back them up first if needed.

### 3. Install the tools the configs use

`.zshrc` expects [sheldon](https://github.com/rossmacarthur/sheldon),
[mise](https://mise.jdx.dev/), and [fzf](https://github.com/junegunn/fzf)
to be installed.

## Updating

```sh
chezmoi update  # git pull + apply
```

Edit files in the source directory (`chezmoi cd`), or run `chezmoi edit <file>`,
then run `chezmoi apply`.

Files listed in `.chezmoiignore` (such as this README) are not copied into `$HOME`.
