Issue: CH340 can not be attached to ttyUSBx

# How to solve
https://askubuntu.com/questions/1403705/dev-ttyusb0-not-present-in-ubuntu-22-04

This happens because of conflict between product IDs (a Braille screen reader and my CH340 based chip). Here is the solution :

Edit /usr/lib/udev/rules.d/85-brltty.rules
Search for this line and comment it out:
ENV{PRODUCT}=="1a86/7523/*", ENV{BRLTTY_BRAILLE_DRIVER}="bm", GOTO="brltty_usb_run"
reboot
More on https://unix.stackexchange.com/questions/670636/unable-to-use-usb-dongle-based-on-usb-serial-converter-chip
