# Mac Dev Playbook

Ansible playbook that sets up my Mac: Homebrew packages and apps, dotfiles and macOS defaults.

Forked from [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) and trimmed down to my own setup. Shell, git and editor config live in [nickwaelkens/dotfiles](https://github.com/nickwaelkens/dotfiles).

## What it does

- Installs Xcode command line tools
- Installs Homebrew packages and casks (see `homebrew_installed_packages` and `homebrew_cask_apps` in [`default.config.yml`](default.config.yml))
- Clones [dotfiles](https://github.com/nickwaelkens/dotfiles) to `~/dev/dotfiles` and symlinks them into `~` (fish, starship, git, vim, `.macos`)
- Runs `~/.macos` to apply macOS defaults
- Optional, off by default: Dock layout, sudoers, gem/npm/pip packages, post-provision tasks

## Install

```sh
xcode-select --install
pip3 install --user ansible
export PATH="$HOME/Library/Python/3.9/bin:/opt/homebrew/bin:$PATH"

git clone git@github.com:nickwaelkens/mac-dev-playbook.git ~/dev/mac-dev-playbook
cd ~/dev/mac-dev-playbook
ansible-galaxy install -r requirements.yml
ansible-playbook main.yml --ask-become-pass
```

If Homebrew steps fail, run `brew doctor`. You may need to accept the Xcode license.

## Usage

Run only some parts with tags:

```sh
ansible-playbook main.yml -K --tags "homebrew,dotfiles"
```

Available tags: `homebrew`, `dotfiles`, `macos`, `dock`, `sudoers`, `extra-packages`, `post`.

To add or remove an app, edit `default.config.yml`. For per-machine overrides, create a `config.yml`, which is gitignored and loaded after the defaults:

```yaml
configure_dock: true
homebrew_cask_apps:
  - firefox
```

## Credits

Based on [mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) by [Jeff Geerling](https://www.jeffgeerling.com/). MIT licensed, see [LICENSE](LICENSE).
