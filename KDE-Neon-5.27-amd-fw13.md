# This is for AMD Ryzen 7040 Series configuration on the Framework Laptop 13 ONLY.

**PLEASE NOTE**: This is an unofficial, community-maintained guide and is not endorsed by Framework.

This guide is written for intermediate to advanced users and is not intended for people new to Linux. Sorry :-)

It is recommended that you [read the announcement about installing Linux on the Ryzen-based Framework 13](https://community.frame.work/t/announcement-linux-on-your-framework-laptop-13-amd-ryzen-7040-series/37367) before following this guide.


## This will:

- Help provide a smoother install process.
- Update your KDE Neon install's packages.
- Install the recommended OEM kernel and provide you with an alert should the OEM kernel needing updating.

## Pre-install

After [downloading](https://neon.kde.org/download) KDE Neon User Edition and selecting to boot from the USB drive, ensure you choose **KDE neon (safe graphics)** for the installer. While the default boot option will probably work fine, you may experience some graphical glitches before switching to the correct kernel.

## First Boot

First boot should get you to the login screen just fine. Once you're at the login screen, make sure you login with the "Plasma (X11)" option (the default), *not Wayland*. The Wayland install currently (as of 2023-10-22) has some graphical glitches until you switch to the correct kernel. (In my case, the screen flashing white on fresh renders.)

In Konsole, run:

```
sudo apt update && sudo apt full-upgrade
sudo apt remove linux-generic-hwe-22.04 linux-hwe-* linux-image-generic linux-image-6.2.0-*
sudo apt install linux-oem-22.04c
sudo apt autoremove
```

When removing the kernels, you may get a warning about this making your system potentially unbootable due to uninstalling the kernel currently being used. If you're worried about this, you can skip the remove step, boot into the OEM kernel from grub, and then run the remove steps.

After this, reboot. After rebooting, check that you are on a 6.1 kernel by running `uname -a`. If you are, you may now use a Wayland session if you choose.

# [Optional] Additional changes

## Fingerprint reader

The fingerprint redaer does not work out of the box. However this can be easily remedied using [Framework's official instructions for Ubuntu](https://github.com/FrameworkComputer/linux-docs/blob/main/13th-gen-fingerprint-reader-firmware.md). Note that Ubuntu 22.04 (and thus the versions of KDE Neon based on it) come with `fwupd` version 1.7.9, so you will need to add the quirk suggested in that file.

After ensuring you have the correct firmware in your fingerprint reader, run `sudo apt install fprintd libpam-fprintd` in Konsole to install the appropriate fingerprint reader software. Once that is installed, you will be able to enrol fingerprints from the Users section of System Settings. However, in order to use your fingerprint to unlock your screen, you will also need to run `sudo pam-auth-update`.

**NOTE:** `pam-auth-update` sets `max-tries=1` and `timeout=10`. Some users may wish to modify these values. If so, you'll need to manually edit `/etc/pam.d/common-auth`

After these changes, you should be able to use your fingerprint reader to unlock your screen, instead of a password for `sudo` commands, and in password dialogs from KDE. Fingerprint based login is not yet supported.


## oem D kernel (6.5 kernel)

While Framework recommends using the OEM C kernel on Ubuntu-based systems, a linux 6.5-based kernel is available in the `linux-oem-22.04d` package and is being tested by some community members. While no known bugs have been found so far, that doesn't guarantee an experience on par with the supported experience.
