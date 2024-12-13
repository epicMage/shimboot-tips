# Shimboot Tips 
Shimboot is a super easy exploit to do, and the benefits are endless, but let's just say Linux is not the easiest of operating systems to work with. I used to have no idea how to work a Debian 12 distro when I first started but I've managed to make everything work up until now. Here are a few things that I learned along my way that I found helpful and wish I had knew when I first started using shimboot.

Note: Please read and follow the instructions. I assume you have a pair of eyes while reading this. Nothing is more frustrating than a user asking a question that is so explicitly answered in the guide. So if you have a question, check the guide again, make sure you have followed all the proper steps, and if finally you find that something doesn't work you can post it in the issues on this repository or go to this discord run by ading2210: https://discord.gg/vddDp326Vs

## Read the official README
Trust me, it helps a lot. It might be confusing at first but its really not that hard. **Just follow the steps in the video/readme and you should be set. Also, a lot of frequently asked questions are probably covered in the official github's FAQ section of the readme.** Definitely watch the video posted on the readme as well, super helpful.
https://github.com/ading2210/shimboot

On first booting into shimboot, you'll want to do a few things:
- Don't forget to run `sudo expand_rootfs` to expand your root file system. It should be the first thing you do.
- On first boot, also remember to connect to a wifi network. You'll need it for the next step.
- Run `sudo apt update` and `sudo apt upgrade` on first boot too, and periodically as well, or before you install something new. **This is especially important if you want to switch the desktop environment, since it will fail unless you have ran these two commands before running `sudo tasksel`** (see "How do I not make shimboot look so primitive?").
-  By default the password is set to "user" and the username is also "user", but you can change this with `sudo passwd user`. One thing to note is that you don't need to set the root password, since the user is the root user (from ading2210).
-  Your time is probably not set to the right time. To fix this, run `sudo dpkg-reconfigure tzdata` on boot. You'll need to figure out what timezone you are in, and the timezones that correspond to your timezone in the `tzdata` data base may be different, for example, `UTC-8`, or `Pacific Standard Time`, is `America/Los_Angeles` in the `tzdata` database. Keep in mind of these things when you set the timezone.
- Function keys are not mapped properly by default. I believe https://github.com/WeirdTreeThing/cros-keyboard-map can fix this, but I'm not entirely sure. You can also mess around with the shortcuts in settings to suit your preferences, since some of them may or may not require function keys and you might want to change them if they are important (like `Alt+F4` to close window, if function keys aren't mapped use something like `Ctrl+Shift+W`).
- In power management (or just power settings in general), change all settings that say "Suspend" to "Lock Screen", unless you want your computer to shut down after a light press of the power button or closing of the lid. Suspend is not supposed to shut down a Linux system, but it doesn't work on shimboot universally (see official shimboot github README).
- Also, chromebooks will restart with `power + refresh`. Avoid using this, as this may cause data corruption (also from ading2210). Instead, use linux's system shutdown functionality.

## How to boot into shimboot after shutting down?
You should enter a screen that says confirm returning to secure mode, and all you need to do is just hit `esc` + `power` + `refresh` all at the same time (basically powerwash) to open the screen that says plug in your usb to get started. 

If you don't want to boot into shimboot, don't do the powerwash part and hit the up arrow (so that the confirm button is highlighted) and hit enter.

## I want to run Roblox so I can play dress to impress and adopt me.
There are a few caveats to this, although it is possible. ~~First is playing these two games specifically~~, and second is that you need to have a late enough kernel version, and third, most importantly, **I have no idea if it works or not**. Anyways, to be able to run roblox on linux, you'll need something called Sober. 

First, run `uname -r` in terminal, if the number it spits out is smaller than `5.11`, then you can't run roblox. No alternative solutions that I know of work. This is a super important step because you will run into error if you try and run this program on an earlier kernel version.

If it is greater, simply follow the instructions here: https://sober.vinegarhq.org/

If you run into an issue like [this](https://github.com/flatpak/flatpak/issues/5944), then simply use the solution I posted at the bottom of the message chain. 

**Note: I have not tested the validity of this program beyond this point. If you run into some other error and you have done all of the above, it might not be possible to run Roblox on shimboot after all. If it doesn't work, I don't have a fix.**

## How can I upgrade my kernel version to play Roblox to play DTI and adopt me with my friends?
You can't upgrade your kernel version, sorry. No buts, no exceptions. If you go ask ading2210, you'll get the same answer.

## How to install VPN?
There are a lot of vpn configurations out there, but I would recommend ProtonVPN because it's decent and it's free. There are two ways you can do this: download the official app, or use OpenVPN configuration. I ran into issues using the official app, while the OpenVPN config works just fine. 

Follow these instructions to obtain ProtonVPN on shimboot (assumes you already have an account setup, so do that first): https://protonvpn.com/support/linux-openvpn/

These instructions are for using OpenVPN file configuration. You can try the official app, it may or may not work for you.

## How do I make shimboot not look so primitive?
Shimboot is not primitive, its the latest version of Debian linux running on your chromebook. It's just the *desktop environment*, or in other words the UI. You can run `sudo tasksel` to configure the desktop environment. Use arrow keys to navigate the CLI, space bar to select/deselect option, tab to switch to the confirm button, then press enter. Restart for changes to take effect.

Note: before restarting, run `sudo autoremove xfce4*` to make the desktop environment you selected to be the default one.

If you do select an option like Cinnamon, or GNOME, make sure you only have one of those options selected. For example, the default is XFCE, but if you want to switch to MATE, then deselect XFCE (press space on it) and use arrow keys to get to MATE and select it. 
Do keep in mind that different desktop environments use different amounts of resources. GNOME is a very user friendly and UI-rich desktop environment, but it's generally quite slow. XFCE is the default, and it looks very basic, but it's faster. I personally use Cinnamon, which has a windows-looking UI and is somewhere in between in terms of performance.

Also, ading2210 recommends you install SSH server and laptop on first boot as well.

## My chromebook battery is draining faster!
Yeah that'll happen. You can install TLP, which is a convenient battery manager for linux in general, and it has significantly improved my battery usage. Also, firefox and chrome in general are quite bad in terms of ram usage. Find what works for you honestly.

To install tlp, run `sudo apt install tlp tlp-rdw` and then run `sudo tlp start`.

There is a 100% workaround, per ading2210, who told me that throttling the cpu (via the use of https://github.com/vagnum08/cpupower-gui) is the way to go. I haven't tried it, but if the developer says it works, it probably works. It obviously has performance implications as well, though.

## How do I install applications?
You can run .appimage files or .deb files. Per the Debian wiki, it is best to avoid running .rpm files on Debian distros. See https://wiki.debian.org/RPM. 

To install .deb files, run `sudo apt install /path/to/deb/file.deb` and replace the last part with the path to the deb file.

AppImage files are a bit weirder, you first have to go to terminal and run `chmod +x /path/to/app.appimage` and then double click the appimage file in file explorer.

## Can I use shimboot to create other shimboots?
Yes, surprisingly, you can. Depending on your chromebook, you will need more than one usb port for this. You can buy a decent hub like this (it's the one I use and it works great): https://a.co/d/dGgS93k. If your chromebook has 2 or more usb ports, you can ignore this.

Once you have shimboot loaded, open terminal and plug in your other flash drive as well. Run `sudo fdisk -l` to see the list of disks plugged in. `/dev/sda` is your shimboot disk and you don't want to be messing with that, so remember to not use that. The last thing you need is to overwrite your own shimboot disk. If you only have one drive plugged in (one drive NOT counting your shimboot drive), it will be `/dev/sdb`. 

Once you have your drive figured out, obtain the appropriate shimboot image (see the official github on figuring out what image you need), whether that be by downloading the image from the official github and unzipping it, or by building the image yourself. Then, run this command:

```
sudo dd if=/path/to/shimboot.bin of=/dev/sd* oflag=direct bs=1M status=progress
```

...replacing `/path/to/shimboot.bin` with the path to your shimboot file and `/dev/sd*` with your drive name (usually `/dev/sdb`).

Important: the file should be the `.bin` file, not the `.zip` file. Extract the `.zip` file first.

Wait for it to finish flashing the image, afterwards, run `sync` and then unplug the drive and you're free to start booting another shimboot off of it!

`dd` can also be used to create various other sorts of bootable drives (like shimboot) in this fashion. You just have to replace the path to shimboot file with the proper image that you want to burn onto the flash drive.
