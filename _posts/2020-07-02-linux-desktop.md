2020 is the year of linux on the desktop. It is known.

I chose to resurrect my old desktop machine. It was originally built in 2013
and got some upgrades in 2016. Right now it has:
- Intel Core i5 processor (never upgraded)
- 256GB SSD (upgraded)
- 500 GB HDD
- 16 GB RAM (upgraded)
Core specs are not bad for basic software development. The problem is the
peripherals:
- Display: VGA and DVI
- Onboard graphics only
- Ethernet and wifi (upgraded)
- No bluetooth
- No HDMI, DP, etc

Mandatory hardware fixes:
- $10 VGA to HDMI adapter
- Accept lack of mouse support
- Plug keyboard into the correct USB port (back 2nd block left, one of
  the only two USB-3 ports on the machine).

See my previous posts on Ubuntu on my laptop for the basic philosophy here.
I really wanted a minimal setup, with text mode only, for development and
study/research. No distractions. This machine can take advantage of actual
modern ergonomic peripherals (monitor, keyboard, etc).

Installation notes:
- Backed up my old stuff to some thumb drives. Most of the old files are on
  the HDD which I'm not planning to modify, but I still backed up (almost)
  everything, just in case.
- I ran into issues (again) here because I foolishly had an minimal install
  disk for the wrong Ubuntu version, 19.04 (disco). I had to burn a new
  minimal install disc for 20.04 LTS (focal) which is the version I meant to
  use. (This was a problem on my laptop too, that required an extra upgrade).
  - Again this was hidden in a [secret download
    directory](http://archive.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/current/legacy-images/netboot/)
- I decided to try the "expert" installer this time, but it seems pretty much
  the same aside from using a tree structure instead of a linear wizard. This
  seemed mostly like an improvement, but in the end I couldn't keep track of
  the order of steps so I ended up going with the basic install again.

Notes when following same software setup from previous posts:
- Correct systemd enable command is
```
$ sudo systemctl enable swap_caps_esc.service
```
- When setting up terminal fonts, my whole monitor and setup is quite
  different. (To be honest, rendering my screen as VGA converted to HDMI is
  probably messing things up a lot.) Some notes:
  - Fixed
    - ok, but kinda ugly. This is the default
    - 8x14
      - This size gets a bit small/squinty. Also, looks "muddy" with some kind
        of blurring/aliasing that's hard to read.
    - 8x15
    - 8x16
      - **This is the default size**
    - 8x18
      - The characters of this font look really ugly ("muddy") at this size
    - Of the Fixed fonts, 8x16 is by far the best. Other sizes mostly have
      strange readability issues.
  - Goha
    - I think this one's pretty ugly -- didn't test in detail.
  - Terminus
    - 6x12
      - This fits **tons** of characters on the screen, but is really
        small/squinty to look at. I might switch to this for serious
        multitasking but it is pretty hard to use most of the time.
    - 8x14
      - This is also kind of muddy looking.
      - Definitely nicer than Fixed 8x14
    - 8x16
      - Definitely a readable size, and nicer than Fixed 8x16
    - 10x20
      - This is highly readable and easy to see. But screen size is starting
        to get a bit limited at this point.
      - Note Terminus 10x20 is what I used on my laptop. But that screen has
        very different size/aspect ratio and also isn't going through VGA->HDMI
        conversion!
      - There is some slight blurring/readability issue with some characters.
    - 11x22
      - Again, highly readable, a bit bigger
    - 12x24
      - This is getting too big -- can't fit much on the screen at this point!
    - Terminus Bold
      - I decided to try this because the line width of Terminus at larger
        sizes was very thin, and I thought this might improve readability.
      - 8x16
        - Definitely too fat/squished compared to regular-weight 8x16
      - 10x20
        - This looks nicer in some ways than regular-weight 10x20.
        - Readability improvement for big characters, but some blockiness/bad
          aesthetics.
      - 11x22
        - This is the smallest size at which the bold stops looking "blocky"
          and starts looking "normal"
        - This size is visibly "big" but uses lots of screen real estate!
      - 12x24
        - This is a **huge** readability improvement over regular-weight 12x24!
        - This is my top choice for a "large print" font!
  - I also noticed my system has special "VGA-optimized" (??) fonts. Let's give
    these a try too! (I am using VGA, after all)
    - VGA
      - 8x16
        - This definitely avoids the blurring issues from using 8x16 on
          Terminus and Fixed fonts! **This is a must have for VGA connectivity!**
        - However this size is a bit blocky
      - 16x28
        - This size is visibly blocky/pixelated and ugly.
    - Terminus Bold VGA
      - 8x16
        - Definitely much less blurred than the regular-weight Terminus 8x16
        - A bit blocky, but definitely workable at 8x16.

Note number of characters per font size on my screen:
-  6x12 => 319x89
-  8x14 => 239x76
-  8x15 => 239x71
-  8x16 => 239x66
-  8x18 => 239x59
- 10x20 => 191x53
- 11x22 => 173x48
- 12x24 => 159x44
Note also that fonts look very different in white-on-black vs black-on-white!
Vision is a very strange phenomenon.


Top choices for comparison:
- Terminus Bold VGA 8x16 ("small")
- Terminus Bold 11x22 ("medium")
- Terminus Bold 12x24 ("big")

In the end I felt most comfortable at 11x22. This is probably an indicator that my eyesight
is getting worse also, but that's a separate issue. Spend more time outside!

- Additional utilities:
  - I skipped powertop (I never use this)
  - Installed git, tmux, vim
  - elinks, wget, curl
  - (sicp) mit-scheme, mit-scheme-dbg, mit-scheme-doc
  - postponed rust and clang for now (since I'm not using these)
  - pdf command line tools all depend on x11 (oops!) so I'm skipping
    these for now!
