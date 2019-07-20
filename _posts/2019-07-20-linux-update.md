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
  layout to Dvorak.
- I like to swap caps lock and escape (I'm a vim user). To do this, build a key
  map using `showkey`, `dumpkeys` and `loadkeys`. My keymap is,
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

#### Terminal environment

It is sadly impossible to configure the console terminal very much. But I tried
my best.
- Use the following command to change terminal settings and rebuild boot image
  as needed.
  ```
    $ sudo dpkg-reconfigure console-setup
  ```
  - Quick note: the live update this uses to change the console on-the-fly
    overrides my fancy keymaps. So they'll no longer be active in this session.
    But they take effect again on reboot, so there's nothing to worry about!
- Change font from "Fixed" (the default) to "Terminus", which supposedly
  reduces eyestrain.
- The default font is pretty small. But my laptop screen is also pretty small.
  Some quick estimates:
    - At Terminus 8x16, 200x56 characters fit on my laptop screen
    - At Terminus 10x20, 144x46
    - At Terminus 11x22, 130x40
    - At Terminus 12x24, 120x37 (the font starts to look pretty ugly at this
      size too)
- I switched the font size to 10x20, which I think is a good compromise between
  readability and actually fitting enough stuff on screen. This is small enough
  to fix 2 windows (of about 70 characters) side by side in tmux.
- I played around with colors too, but with only 8 colors to choose
  from there just isn't much point. Furthermore it seems that the meanings of
  specific color descriptions are baked into the kernel, so (without
  recompiling) there's no way to change colors. Commands like `setterm` get
  reverted as soon as any command wants to use colors within its output.
- I'll try tweaking colors for specific commands once I have more stuff
  installed.

#### Software

(Work in progress, will update)

#### References
- [SuperUser: How to change input keyboard layout while in
  console](https://superuser.com/questions/404457/how-to-change-input-keyboard-layout-while-in-console)
- [SuperUser: How to change console keymap in
  Linux](https://superuser.com/questions/290115/how-to-change-console-keymap-in-linux)
- [Unix & Linux SE: How to write a startup script for
  systemd](https://unix.stackexchange.com/questions/47695/how-to-write-startup-script-for-systemd#47715)
- [Unix & Linux SE: Can I change the font of
  terminal](https://unix.stackexchange.com/questions/49779/can-i-change-the-font-of-terminal)
- [Change console fonts in Ubuntu
  server](https://www.tecmint.com/change-console-fonts-in-ubuntu-server/)
- [SuperUser: Is there a way to alter the colors used in tty consoles on
  Linux](https://superuser.com/questions/314035/is-there-a-way-to-alter-the-colors-used-in-tty-consoles-on-linux)
- [Unix & Linux SE: Colorizing your terminal and shell
  environment](https://unix.stackexchange.com/questions/148/colorizing-your-terminal-and-shell-environment)
