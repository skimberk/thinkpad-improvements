# Thinkpad Improvement

I just bought (in August 2020) a used Thinkpad T440P and installed Ubuntu. I'm going to try and record any solutions to problems/general improvements here.

## Screen tearing when scrolling

When scrolling there would be little lines of lag across the screen. This was especially noticeable when I was scrolling through text in Firefox. I followed the instructions here which solved the issue: https://askubuntu.com/a/1111020

## Touchpad kinda wonky

**At first (with `libinput`):** The touchpad just didn't work that well. It wouldn't right click when I physically pressed the touchpad with two fingers and scrolling was a little janky. It would also right click sometimes when I was trying to select text (and therefore was clicking with two fingers on the touchpad).

**Tried synaptics:** I followed the instructions in this answer, and it helped in some respects (scrolling is much better) but it also made some things worse (the cursors jumps around when I remove one finger after having clicked with two): https://askubuntu.com/a/1035863 Update: upon further use, it's pretty bad. Scrolling is good, but a lot of things are pretty bad. One thing I didn't realize I needed until it was gone was thumb rejection (I rest my thump on the trackpad to click while tracking with my index finger).

**Trying mtrack:** I'm going to try following the instructions here: https://howchoo.com/g/mdy0ngziogm/the-perfect-almost-touchpad-settings-on-linux-2

*WARNING: This resulted in my track pad no longer working. I had to open terminal using `ctrl-shift-T`, removed the conf file, then restarted with these instructions: https://askubuntu.com/a/997223*

Steps:

1. `sudo apt install pkg-config make xutils-dev libtool xserver-xorg-dev libx11-dev libxi-dev libxrandr-dev libxinerama-dev libudev-dev`
2. `sudo apt-get install libmtdev`
3. Followed the instruction in the blog post. Make sure to `./configure` after installing any dependencies before running `make`!
