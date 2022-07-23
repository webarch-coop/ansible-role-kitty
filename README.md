# Kitty

Ansible role to install Kitty from GitHub.

This role is designed to be run without `become`, you might want to copy the following to `/usr/share/applications/kitty.desktop` and `chmod 755` it:

```
[Desktop Entry]
Version=1.0
Type=Application
Name=kitty
GenericName=Terminal emulator
Comment=Fast, feature-rich, GPU based terminal
TryExec=kitty
Exec=/home/$USER/.local/bin/kitty
Icon=/home/$USER/.local/share/icons/hicolor/256x256/apps/kitty.png
Categories=System;TerminalEmulator;
```

And also create a `/usr/local/bin/kitty` file and `chmod 755` it:

```bash
#!/usr/bin/env bash

if [[ -f "/home/${USER}/.local/bin/kitty" ]]; then
  /home/${USER}/.local/bin/kitty
elif [[ -f "/home/${USER}//bin/kitty" ]]; then
  /home/${USER}//bin/kitty
elif [[ -f /usr/local/bin/kitty ]]; then
  /usr/local/bin/kitty
elif [[ -f /usr/bin/kitty ]]; then
  /usr/bin/kitty
else
  echo "Sorry no kitty found"
  exit 1
fi
```
