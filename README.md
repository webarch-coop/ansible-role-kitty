# Kitty

Ansible role to install Kitty from GitHub.

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
