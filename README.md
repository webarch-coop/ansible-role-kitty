# Kitty

Ansible role to install and update to the [latest](https://github.com/kovidgoyal/kitty/releases/latest) version of [Kitty](https://github.com/kovidgoyal/kitty) in `~/.local`.

This role is designed to be run without `become`, as a non-root user, you might want to copy the following to `/usr/share/applications/kitty.desktop` and `chmod 755` it:

```
[Desktop Entry]
Version=1.0
Type=Application
Name=kitty
GenericName=Terminal emulator
Comment=Fast, feature-rich, GPU based terminal
TryExec=kitty
Exec=/$HOME/.local/bin/kitty
Icon=/$HOME/.local/share/icons/hicolor/256x256/apps/kitty.png
Categories=System;TerminalEmulator;
```

And also create a `/usr/local/bin/kitty` file and `chmod 755` it:

```bash
#!/usr/bin/env bash

if [[ -f "/${HOME}/.local/bin/kitty" ]]; then
  /${HOME}/.local/bin/kitty
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
