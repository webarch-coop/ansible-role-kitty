# Webarchitects Kitty Ansible Role

[![pipeline status](https://git.coop/webarch/kitty/badges/main/pipeline.svg)](https://git.coop/webarch/kitty/-/commits/main)

Ansible role to install update [Kitty](https://github.com/kovidgoyal/kitty) in `~/.local` on Debian and Ubuntu.

This repo can be used to install and update the [latest version](https://github.com/kovidgoyal/kitty/releases/latest), or the [nightly version](https://github.com/kovidgoyal/kitty/releases/tag/nightly) or a [specific version](https://github.com/kovidgoyal/kitty/releases) of [Kitty](https://github.com/kovidgoyal/kitty) equal to or newer than `0.20.3` (earlier versions did not have GPG signatures).

## Usage

The suggested method for using this role is via the [localhost repo](https://git.coop/webarch/localhost) which contains a [kitty.sh](https://git.coop/webarch/localhost/-/blob/main/kitty.sh) script that will download this role and run it, for example:

```bash
git clone https://git.coop/webarch/localhost.git
cd localhost
./kitty.sh --check
./kitty.sh
```

This role is designed to be run by a non-root user, it will install Kitty to `~/.local/bin`, if `~/.local/bin` is not in your `$PATH` environmental variable add the following to your `~/.bash_profile` or whichever file sets your `$PATH` environmental variable when you login:

```bash
PATH="${HOME}/.local/bin:${PATH}"
export PATH="${PATH}"
```

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

| Variable name        | Default value                             | Comment                                                                   |
|----------------------|-------------------------------------------|---------------------------------------------------------------------------|
| `kitty`              | `true`                                    | If this variable is set to `false` all tasks in this role will be skipped |
| `kitty_bin`          | `{{ ansible_env.HOME }}/.local/bin`       | The directory in which the `kitty` binary resides                         |
| `kitty_local`        | `{{ ansible_env.HOME }}/.local/kitty.app` | The directory that the Kitty archive is extracted into                    |
| `kitty_tmp`          | `{{ ansible_env.HOME }}/tmp`              | The directory that the Kitty archive and GPG signature to downloaded into |
| `kitty_version`      | `latest`                                  | Valid options are `latest`, `nightly` or a version number, eg `0.20.3`    |

The variable `ansible_env.HOME` is the `$HOME` directory of the user running Ansible.

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

This role can also be used with the [localhost repo](https://git.coop/webarch/localhost) to install `kitty` locally however not that it would need some changes so that it doesn't prompt for the `sudo` password and then run as `root`.

## Copyright

Copyright 2022-2023 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
