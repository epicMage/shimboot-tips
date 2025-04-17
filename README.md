# Shimboot Tips Again
<h6>my own version of [https://github.com/Edward358-AI/shimboot-tips]. Pretty much nothing was changed, though some things are updated. I'm open to this being merged with the OG. </h6>

**I refers to** https://github.com/Edward358-AI

Shimboot is a super easy exploit to do, and the benefits are endless, but let's just say Linux is not the easiest of operating systems to work with. I used to have no idea how to work a Debian 12 distro when I first started but I've managed to make everything work up until now. Here are a few things that I learned along my way that I found helpful and wish I had knew when I first started using shimboot.

Note: Please read and follow the instructions. I assume you have a pair of eyes while reading this. Nothing is more frustrating than a user asking a question that is so explicitly answered in the guide. So if you have a question, check the guide again, make sure you have followed all the proper steps, and if finally you find that something doesn't work you can post it in the issues on this repository or go to this discord run by ading2210: https://discord.gg/vddDp326Vs

## Read the official README
Trust me, it helps a lot. It might be confusing at first but its really not that hard. **Just follow the steps in the video/readme and you should be set. Also, a lot of frequently asked questions are probably covered in the official github's FAQ section of the readme.** Definitely watch the video posted on the readme as well, super helpful.
https://github.com/ading2210/shimboot

On first booting into shimboot, you'll want to do a few things:

Don't forget to run
```
sudo expand_rootfs
```
to expand your root file system. It should be the first thing you do.

On first boot, also remember to connect to a wifi network. You'll need it for the next step.

Run this on first boot:
```
sudo apt update && sudo apt upgrade
``` 
And periodically as well, or before you install something new. **This is especially important if you want to switch the desktop environment, since it will fail unless you have ran these two commands before running `sudo tasksel`** (see "How do I not make shimboot look so primitive?").

By default the password is set to "user" and the username is also "user", but you can change this with 
```
sudo passwd user
```
One thing to note is that you don't need to set the root password, since the user is the root user (from ading2210).

Function keys are not mapped properly by default. I believe https://github.com/WeirdTreeThing/cros-keyboard-map can fix this, but I'm not entirely sure. You can also mess around with the shortcuts in settings to suit your preferences, since some of them may or may not require function keys and you might want to change them if they are important (like `Alt+F4` to close window, if function keys aren't mapped use something like `Ctrl+Shift+W`).

In power management (or just power settings in general), change all settings that say "Suspend" to "Lock Screen", unless you want your computer to shut down after a light press of the power button or closing of the lid. Suspend is not supposed to shut down a Linux system, but it doesn't work on shimboot universally (see official shimboot github README).

Also, chrorescues will restart with `power + refresh`. Avoid using this, as this may cause data corruption (also from ading2210). Instead, use linux's system shutdown functionality. Use `reboot` to shut down your system.

Another helpful tip might be that `Ctrl+Shift+V` is paste in terminal, and to copy to clipboard select text and right-click to open a menu and hit copy to copy it.

## How to boot into shimboot after shutting down?
You should enter a screen that says confirm returning to secure mode, and all you need to do is just hit `esc` + `power` + `refresh` all at the same time (basically powerwash) to open the screen that says plug in your usb to get started. 

If you don't want to boot into shimboot, don't do the powerwash part and hit the up arrow (so that the confirm button is highlighted) and hit enter.

## Can I still use shimboot even if I switch computers?
Yes, just follow the steps to get to the part where you plug in the flash drive. Basically just go to the official shimboot readme, watch the video up until the part where he plugs in the flash drive, then you can exit and begin using shimboot like normal.

Note that booting up in developer mode causes all local data on your chromebook to be lost, so if it is someone else's chromebook, be sure to get their permission first.

## Something isn't installing when I type 'sudo apt install package'
Check your spelling. Run 
```
sudo apt update && sudo apt upgrade
```
if necessary. Otherwise, if a problem persists, go search it up.

## The time displayed is incorrect.
To fix this, run 
```
sudo dpkg-reconfigure tzdata
```
You'll need to figure out what timezone you are in, and the timezones that correspond to your timezone in the `tzdata` data base may be different, for example, `UTC-8`, or `Pacific Standard Time`, is `America/Los_Angeles` in the `tzdata` database. Keep in mind of these things when you set the timezone. 

Also a nice to know is that if you use Cinnamon or Gnome you can instead use an alternate solution. Run this command in terminal:
```
sudo apt install ntp
``` 
and go to settings > date and time and then enable the switch saying `Use Network Time` and then change the timezone to the preferred one.

## How do I change the username? I don't like my current username.
You need to be not logged in to change username, which does pose an issue. The solution directly using shimboot is on the shimboot loading screen, instead of selecting option 3, type `rescue <option>` to enter rescue shell, where <option> is your boot partition, usually 3, and type each of the following commands separately:
```
sudo usermod -l newUsername oldUsername
sudo usermod -d /home/newHomeDir -m newUsername
```
The second one is optional, it just changes the home directory folder name to match your username. The first one is what actually changes your username.

Then of course, type `exit` after you type out these commands to reboot.

Note if you are on earlier versions of shimboot they might not have rescue shell.

## My touchpad isn't right-clicking when I use two fingers.
By default the Debian distro on shimboot assumes the bottom right corner of your touchpad to right click. To fix this behavior, there are a series of steps you need to follow:

Note: If you are using Cinnamon or Gnome though, you don't have to do all that below. You can just go into settings and switch the behavior to multiple fingers for right and middle click. On XFCE, you'll have to do that!

First run 
```
sudo apt install xinput
```
After that's finished, run 
```
xinput
```
to see a list of devices. You should see something like this:
```
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ GXTP7288:00 27C6:01F5 Mouse             	id=7	[slave  pointer  (2)]
⎜   ↳ GXTP7288:00 27C6:01F5 Touchpad          	id=8	[slave  pointer  (2)]
⎜   ↳ Elan Touchscreen                        	id=9	[slave  pointer  (2)]
...
```
Find the touchpad. In my case, the touchpad is the 4th item in the list, and its `id` is `8`. Remember this number since you'll need it.

Next, run 
```
xinput list-props #
```
...replacing `#` with your touchpad id number. Scroll down until you get to the property that says this:
```
libinput Click Method Enabled (351):	1, 0
```
I'm not sure if the number in parentheses is the same on your shimboot, but just remember the number in paretheses. If the two numbers following the property listing say `1, 0`, then run the next piece of code. If it says `0, 1`, then yeah I'm not sure why two finger right click is not working. Probably get a new touchpad at that point.

If it does say `1, 0`, like in the block of code above, then you can run the following command:
```
xinput set-prop # ### 0 1
```
Replacing the first `#` with your touchpad device `id` (mine was `8`) and the `###` with the number in parentheses from the step above. This will change your touchpad behavior to two finger right click.

This change is not permanent, however, and to make it permanent, create an `.xsessionrc` file:
```
cat > .xsessionrc
xinput set-prop # ### 0 1
```
Make sure the xinput command is the same command you ran in the previous step, replace the `#`'s accordingly. To finish, press `Ctrl+D` to exit the editor and now this command will be run on every login.

## I want to run Roblox so I can play dress to impress and adopt me.
There are a few caveats to this, although it is possible (tested to some degree, in actuality may or may not be). ~~First is playing these two games specifically~~, and second is that you need to have a late enough kernel version, and third, most importantly, **I have no idea if it works or not**. Anyways, to be able to run roblox on linux, you'll need something called Sober. 

First, run 
```
uname -r
```
in terminal, if the number it spits out is smaller than `5.11`, then you can't run roblox. No alternative solutions that I know of work. This is a super important step because you will run into error if you try and run this program on an earlier kernel version.

If it is greater, simply follow the instructions here: https://sober.vinegarhq.org/. Be sure to check if flatpak is installed correctly/properly.

If you run into an issue like [this](https://github.com/flatpak/flatpak/issues/5944), then simply use the solution (I)[https://github.com/Edward358-AI] posted at the bottom of the message chain. 

**Note: I have not tested the validity of this program beyond this point. If you run into some other error and you have done all of the above, it might not be possible to run Roblox on shimboot after all. If it doesn't work, I don't have a fix.**

## How can I upgrade my kernel version to play Roblox to play DTI and adopt me with my friends?
You can't upgrade your kernel version, sorry. No buts, no exceptions. If you go ask ading2210, you'll get the same answer.

## How to install VPN?
There are a lot of VPN configurations out there, but I would recommend ProtonVPN because it's decent and it's free. There are two ways you can do this: download the official app, or use OpenVPN configuration. I ran into issues using the official app, while the OpenVPN config works just fine. 

Follow these instructions to obtain ProtonVPN on shimboot (assumes you already have an account setup, so do that first): https://protonvpn.com/support/linux-openvpn/

These instructions are for using OpenVPN file configuration. You can try the official app, it may or may not work for you.

## How do I make shimboot not look so primitive?
Shimboot is not primitive, its the latest version of Debian linux running on your chromebook. It's just the *desktop environment*, or in other words the UI. You can run
```
sudo tasksel
```
to configure the desktop environment. Use arrow keys to navigate the CLI, space bar to select/deselect option, tab to switch to the confirm button, then press enter. Restart for changes to take effect.

Note: before restarting, run 
```
sudo apt-get autoremove xfce4*
```
to make the desktop environment you selected to be the default one. This removes everything that has xfce4 at the start of it's title. 

If you do select an option like Cinnamon, or GNOME, make sure you only have one of those options selected. For example, the default is XFCE, but if you want to switch to MATE, then deselect XFCE (press space on it) and use arrow keys to get to MATE and select it. 
Do keep in mind that different desktop environments use different amounts of resources. GNOME is a very user friendly and UI-rich desktop environment, but it's generally quite slow. XFCE is the default, and it looks very basic, but it's faster. I personally use Cinnamon, which has a windows-looking UI and is a bit lighter than GNOME (but still not as light as XFCE).

Also, ading2210 recommends you install `SSH server` and `laptop` on first boot as well so when you run `sudo tasksel` you'll want to check those options as well.

## My chromebook battery is draining faster!
Yeah that'll happen. You can install TLP, which is a convenient battery manager for linux in general, and it has significantly improved my battery usage. Also, firefox and chrome in general are quite bad in terms of ram usage. Find what works for you honestly.

To install tlp, run 
```
sudo apt install tlp tlp-rdw
```
and then run 
```
sudo tlp start
```

There is a 100% workaround, per ading2210, who told me that throttling the cpu (via the use of https://github.com/vagnum08/cpupower-gui) is the way to go. I haven't tried it, but if the developer says it works, it probably works. It obviously has performance implications as well, though.

## How do I install applications?
You can run .appimage files or .deb files. Per the Debian wiki, it is best to avoid running .rpm files on Debian distros. See https://wiki.debian.org/RPM. 

To install .deb files, run 
```
sudo apt install /path/to/deb/file.deb
```
and replace the last part with the path to the deb file.

AppImage files are a bit weirder, you first have to go to terminal and run 
```
chmod +x /path/to/app.appimage
```
in order to make it an executable and then double click the appimage file in file explorer.

## Can I use shimboot to create other shimboots?
Yes, surprisingly, you can. Depending on your chromebook, you will need more than one usb port for this. You can buy a decent hub like this (it's the one (I)[https://github.com/Edward358-AI] use and it works great): https://a.co/d/dGgS93k. If your chromebook has 2 or more usb ports, you can ignore this.

Once you have shimboot loaded, open terminal and plug in your other flash drive as well. Run 
```
sudo fdisk -l
```
to see the list of disks plugged in. `/dev/sda` is your shimboot disk and you don't want to be messing with that, so remember to not use that. One thing you probably don't want is to overwrite your current shimboot drive. If you only have one drive plugged in (one drive NOT counting your shimboot drive), it will be `/dev/sdb`. 

Once you have your drive figured out, obtain the appropriate shimboot image (see the official github on figuring out what image you need), whether that be by downloading the image from the official github and unzipping it, or by building the image yourself. Then, run this command:

```
sudo dd if=/path/to/shimboot.bin of=/dev/sd* oflag=direct bs=1M status=progress
```

...replacing `/path/to/shimboot.bin` with the path to your shimboot file and `/dev/sd*` with your drive name (usually `/dev/sdb`).

Important: the file should be the `.bin` file, not the `.zip` file. Extract the `.zip` file first.

Wait for it to finish flashing the image, afterwards, run 
```
sync
```
and then unplug the drive and you're free to start booting another shimboot off of it!

`dd` can also be used to create various other sorts of bootable drives (like shimboot) in this fashion. You just have to replace the path to shimboot file with the proper image that you want to burn onto the flash drive.

## How do I input in Chinese?
Okay granted I hardly think most of you will use it, but (I'm)[https://github.com/Edward358-AI] Chinese and I use it from time to time, so in case there are weird people like me out there, you can learn how to make pinyin work. The most common solution is to run 
```
sudo apt install ibus-libpinyin
```
and then after that installs, run 
```
ibus-setup
```
and change the shortcut to change the input method (I'm accustomed to `Ctrl+Space` but that's just me), and the most important part is to obviously add the input method itself, so go to input methods tab and add intelligent pinyin (if you are using pinyin) or other input methods as necessary. Use the shortcut to switch between English and Chinese, and voila, there you go, pinyin on linux.
