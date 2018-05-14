# Autoconfigure your TypeMatrix Keyboard in Xorg

Problem: You use a different layout on your laptop's keyboard than on your TypeMatrix keyboard and you want a custom layout to be configured automatically when this keyboard is plugged in.

The solution is to place an Xorg config file like the one provided in the `xorg.conf.d` directory.

## Udev

One could use Udev for the same purpose, but this solution is simpler because:

- Udev has "no" access to your Xorg session without providing it your `$HOME/.Xauthority` token, and then there is no need for a custom Udev rule and a script in to run `setxkbmap` 
- On the lock screen, the alternate layout is not recognize because the lock screen is "outside" your session

If you are curious, here is a recipe from [Cédric Gatay](https://twitter.com/cedric_gatay) using Udev: http://www.bloggure.info/AutoApplyKeymapUdev/

## Sample config file

This configures a TypeMatrix keyboard for the [Colemak layout](https://en.wikipedia.org/wiki/Colemak):

```
Section "InputClass"
    Identifier "TypeMatrix 2030 USB Keyboard"
    MatchIsKeyboard "on"
    MatchUSBID "1e54:2030"

    Driver "evdev"
    Option "XkbRules"   "evdev"
    Option "XkbModel"   "pc105"
    Option "XkbLayout"  "us"
    Option "XkbVariant" "colemak"
    Option "XkbOptions" "caps:escape"
EndSection
```

Optionally, you can rebind `caps` to `escape`:

```
    Option "XkbOptions" "caps:escape"
```

## Install

This copies the provided [10-typematrix.conf](10-typematrix.conf) to your system:

```bash
sudo mkdir -p /etc/X11/xorg.conf.d/
sudo cp 10-typematrix.conf /etc/X11/xorg.conf.d/
# You then need to restart your session
```
