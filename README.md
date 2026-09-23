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

### 3. Tools are installed by mise

The only tool you need to bootstrap is [mise](https://mise.jdx.dev/).
On `chezmoi apply`, the `run_onchange_install-tools.sh` script installs mise
into `~/.local/bin` if it is missing, then runs `mise install` for every tool in
`~/.config/mise/config.toml` (neovim, sheldon, gh, fzf, bw, kubectl, node, ...).
The script runs again whenever that config changes.

To add a tool, add it to `private_dot_config/mise/config.toml` and run
`chezmoi apply`. Tools that mise has no short name for can use the aqua
backend, e.g. `"aqua:rossmacarthur/sheldon" = "0.8.5"`.

Pin exact versions (not `latest`) so that Renovate can open update PRs.
Renovate groups minor/patch updates into one weekly PR and auto-merges it;
major updates get their own PR. Run `chezmoi update` to pull and install them.

## Updating

```sh
chezmoi update  # git pull + apply
```

Edit files in the source directory (`chezmoi cd`), or run `chezmoi edit <file>`,
then run `chezmoi apply`.

Files listed in `.chezmoiignore` (such as this README) are not copied into `$HOME`.
