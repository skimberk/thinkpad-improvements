# Thinkpad Improvement

I just bought (in August 2020) a used Thinkpad T440P and installed Ubuntu. I'm going to try and record any solutions to problems/general improvements here.

## Screen tearing when scrolling

When scrolling there would be little lines of lag across the screen. This was especially noticeable when I was scrolling through text in Firefox. I followed the instructions here which solved the issue: https://askubuntu.com/a/1111020

In case that link stops working, in summary, I created the file `/etc/modprobe.d/zz-nvidia-modeset.conf` and added `options nvidia_drm modeset=1`, then ran `sudo update-initramfs -u` and rebooted.

## Firefox kinetic scrolling

One of my biggest issues with `libinput` is the lack of kinetic scrolling. It seems it can be supported by individual applications, though, and can be enabled in firefox by using the `MOZ_USE_XINPUT2=1` env variable.

You can configure scroll speed in `about:config` by changing the `mousewheel.default.delta_multiplier_` variables. Make sure `apz.gtk.kinetic_scroll.enabled` is `true`.

## Disabling trackpoint

The trackpoint was moving my cursor around while I was typing. I disabled the trackpoint completely with the command:

```
xinput set-prop "TPPS/2 IBM TrackPoint" "Device Enabled" 0
```

## Disabling `PgUp` and `PgDn` (Page Up and Page Down) keys

This is mostly my fault, but I kept accidentally hitting `PgUp` and `PgDn` when I meant to hit one of the arrow keys. To disable this, I first had to find the keycodes. I ran `xev` and clicked the keys, and it displayed the relevant keycodes. It indicated the keycodes as `112` and `117`. To double check, I ran `xmodmap -pke` and was told that those keys corresponded with `Prior` and `Next`, which checks out. This is what that output looked like:

```
...
keycode 109 = Linefeed NoSymbol Linefeed
keycode 110 = Home NoSymbol Home
keycode 111 = Up NoSymbol Up
keycode 112 = Prior NoSymbol Prior
keycode 113 = Left NoSymbol Left
keycode 114 = Right NoSymbol Right
keycode 115 = End NoSymbol End
keycode 116 = Down NoSymbol Down
keycode 117 = Next NoSymbol Next
keycode 118 = Insert NoSymbol Insert
keycode 119 = Delete NoSymbol Delete
...
```

I disabled the keys (temporarily) by running:

```
xmodmap -e 'keycode 112 = 0x0000'
xmodmap -e 'keycode 117 = 0x0000'
```

To make the change permanent, I followed these steps: https://unix.stackexchange.com/a/520756. So, I ran `g` then copied the following into the file:

```
#!/bin/bash

case $1 in
    pre)
        exit 0
    ;;
    post)
        export DISPLAY=:0
        sleep 10
        xmodmap -e 'keycode 112 = 0x0000'
        xmodmap -e 'keycode 117 = 0x0000'
    ;;
esac
```


## Touchpad kinda wonky

**At first (with `libinput`):** The touchpad just didn't work that well. It wouldn't right click when I physically pressed the touchpad with two fingers and scrolling was a little janky. It would also right click sometimes when I was trying to select text (and therefore was clicking with two fingers on the touchpad).

**Tried synaptics:** I followed the instructions in this answer, and it helped in some respects (scrolling is much better) but it also made some things worse (the cursors jumps around when I remove one finger after having clicked with two): https://askubuntu.com/a/1035863 Update: upon further use, it's pretty bad. Scrolling is good, but a lot of things are pretty bad. One thing I didn't realize I needed until it was gone was thumb rejection (I rest my thump on the trackpad to click while tracking with my index finger).

**Trying mtrack:** I'm going to try following the instructions here: https://howchoo.com/g/mdy0ngziogm/the-perfect-almost-touchpad-settings-on-linux-2

*WARNING: This resulted in my track pad no longer working. I had to open terminal using `ctrl-shift-T`, removed the conf file, then restarted with these instructions: https://askubuntu.com/a/997223*

Steps:

1. `sudo apt install pkg-config make xutils-dev libtool xserver-xorg-dev libx11-dev libxi-dev libxrandr-dev libxinerama-dev libudev-dev`
2. `sudo apt-get install libmtdev`
3. Followed the instruction in the blog post. Make sure to `./configure` after installing any dependencies before running `make`!
