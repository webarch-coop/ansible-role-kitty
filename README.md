# Kitty

Ansible role to install update [Kitty](https://github.com/kovidgoyal/kitty) in `~/.local`, tested on Debian, will no doubt work on Ubuntu.

This repo can be used to install and update the [latest version](https://github.com/kovidgoyal/kitty/releases/latest), or the [nightly version](https://github.com/kovidgoyal/kitty/releases/tag/nightly) or a [specific version](https://github.com/kovidgoyal/kitty/releases) of [Kitty](https://github.com/kovidgoyal/kitty) equal to or newer than `0.20.3` (earlier versions did not have GPG signatures).

## Defaults

There are four [default variables](defaults/main.yml):

| Variable name        | Default value    | Comment                                                                   |
|----------------------|------------------|---------------------------------------------------------------------------|
| `kitty`              | `true`           | If this variable is set to `false` all tasks in this role will be skipped |
| `kitty_bin`          | `~/.local/bin`   | The directory in which the `kitty` binary resides                         |
| `kitty_local`        | `~/.local`       | The directory that the Kitty archive is extracted into                    |
| `kitty_tmp`          | `~/tmp`          | The directory that the Kitty archive and GPG signature to downloaded into |
| `kitty_version`      | `latest`         | Valid options are `latest`, `nightly` or a version number, eg `0.20.3`    |

## Using this role

The minimal Ansible configuration needed to use this role is a directory containing a `hosts.yml` file containing:

```yml
---
all:
  children:
    localhosts:
      hosts:
        localhost:
...
```

A `localhost.yml` file containing:

```yml
--
- name: Install packages on the localhost using Ansible
  hosts:
    - localhosts
  connection: local
  vars:
    kitty_version: nightly
  roles:
    - kitty
...
```

And a `requirements.yml` file containing:

```yml
---
- name: kitty
  src: https://github.com/webarch-coop/ansible-role-kitty.git
  version: main
  scm: git
...
```

And an `ansible.cfg` file containing:

```ini
[defaults]
inventory = hosts.yml
roles_path = galaxy/roles
```

Then to install Kitty run the following comands:

```bash
ansible-galaxy install -r requirements.yml
ansible-playbook localhost.yml
```

## Desktop icon and PATH

This role is designed to be run without `become`, as a non-root user, you might want to copy the following to `/usr/share/applications/kitty.desktop` and `chmod 755` it:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=kitty
GenericName=Terminal emulator
Comment=Fast, feature-rich, GPU based terminal
TryExec=kitty
Exec=$HOME/.local/bin/kitty
Icon=$HOME/.local/share/icons/hicolor/256x256/apps/kitty.png
Categories=System;TerminalEmulator;
```

And also create a `/usr/local/bin/kitty` file and `chmod 755` it so that `kitty` is in your `$PATH`:

```bash
#!/usr/bin/env bash

if [[ -f "${HOME}/.local/bin/kitty" ]]; then
  ${HOME}/.local/bin/kitty
else
  echo "Sorry, kitty could not be found"
  exit 1
fi
```

## Remote servers

Install the [kitty-terminfo](https://packages.debian.org/search?keywords=kitty-terminfo) package and see [the SSH documentation](https://sw.kovidgoyal.net/kitty/kittens/ssh/).

## Config tweaks

To ensure that [ctrl+shift+v goes to Vim](https://github.com/kovidgoyal/kitty/discussions/5003#discussioncomment-2617442) add the following to `~/.config/kitty/kitty.conf`:

```
map ctrl+shift+v send_text all \x16
```

## Repo

The primary URL of this repo is [`https://git.coop/webarch/kitty`](https://git.coop/webarch/kitty) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-kitty) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/kitty).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/kitty/-/releases).

This role can also be used with the [localhost repo](https://git.coop/webarch/localhost) to install `kitty` locally.
