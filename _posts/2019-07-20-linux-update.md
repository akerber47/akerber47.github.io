---
title: Configuring a minimal Linux dev environment
---

**Current total boot time: 22 seconds.**
(from BIOS load to full boot: less than 10 seconds).

#### Goals
- Install a keyboard and terminal environment I can work with.
- Install software I need.
- Keep things *extremely* lightweight and "boring" (because boring gets the job
  done, and exciting makes things more like a toy).

#### Keyboard
- Use `sudo dpkg-reconfigure keyboard-configuration` to change system keyboard
  layout.
- Build a key map using `showkey`, `dumpkeys` and `loadkeys`. My keymap is,
  fundamentally:
  ```
    keymaps 0-127
    keycode  58 = Escape
    keycode   1 = CtrlL_Lock
  ```
  (this is not quite it, because I also changed all the Meta keys + Caps Lock
  to map to Meta_Escape. But I'll probably never use this anyway.)
  - Run a quick one-time command to apply the keymap and check that it works.
    Note that this will not persist between sessions:
  ```
    $ sudo loadkeys swap_caps_esc.kmap
  ```
- Put the keymap somewhere global:
  ```
    $ sudo mkdir -p /usr/share/keymaps/
    $ sudo mv swap_caps_esc.kmap /usr/share/keymaps/
    $ sudo chown root /usr/share/keymaps/swap_caps_esc.kmap
    $ sudo chgrp root /usr/share/keymaps/swap_caps_esc.kmap
  ```
- Now we just need to load it every time the system starts. Unfortunately, now
  that systemd is a thing, getting this to run is a bit more involved than it
  was before. We'll need a systemd service.
  - Create the service file:
  ```
    $ sudo vi /etc/systemd/system/swap_caps_esc.service
  ```
  - Edit it as follows:
  ```
    [Unit]
    Description=Swap CapsLock and Escape

    [Service]
    Type=oneshot
    ExecStart=/bin/sh -c "/usr/bin/loadkeys /usr/share/keymaps/swap_caps_esc.kmap"

    [Install]
    WantedBy=multi-user.target
  ```
  - Enable the service:
  ```
    $ sudo systemd enable swap_caps_esc.service
  ```
  - Reboot the machine to check that it works!


#### References
- [SuperUser: How to change input keyboard layout while in
  console](https://superuser.com/questions/404457/how-to-change-input-keyboard-layout-while-in-console)
- [SuperUser: How to change console keymap in
  Linux](https://superuser.com/questions/290115/how-to-change-console-keymap-in-linux)
- [Unix & Linux SE: How to write a startup script for
  systemd](https://unix.stackexchange.com/questions/47695/how-to-write-startup-script-for-systemd#47715)
