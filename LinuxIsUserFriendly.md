# Secondary SSD not mounting on startup

In Terminal run:

`lsblk -o PATH,LABEL,SIZE,RO,TYPE,MOUNTPOINT,UUID,MODEL`

To display a list of all mount points. Locate the physical device by checking MODEL, SIZE and LABEL, then note the UUID.

Run:

`sudo nano /etc/fstab`

Add the following:

    UUID=[uuid] $mountpoint ext4 defaults 0 0

Ctrl+X -> Y to save and exit.

Reboot the computer. The drive should be mounted.

# Bluetooth device connection failing

[source](https://askubuntu.com/questions/1235456/issues-with-bluetooth-mouse-in-ubuntu-20-04)

## Solution

`sudo nano /etc/default/grub`

Completely replace line:

    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

With:

    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash btusb.enable_autosuspend=n usbcore.autosuspend=-1 usbcore.autosuspend_delay_ms=-1"

Ctrl+X -> Y to save and exit.

Run `sudo update-grub`.

# Bluetooth drivers loading slowly

Bluetooth devices taking a couple of seconds after logging in to start working.

## Solution

[Thanks to /u/floofly on r/linux4noobs for this!](https://www.reddit.com/r/linux4noobs/comments/1k9rapk/comment/mpgfifn/)

> Assuming you're using systemd as you innit system. The following will change it so your bluetooth will initialise before the GUI.
>
> `sudo systemctl edit bluetooth.service`
>
> change:
>
>     [Unit]
>     
>     Before=graphical.target

## Noob friendly

### Is my innit system systemd?

Run `systemctl --version` in Terminal. First line should read something like `systemd 257 (257.5-2-arch)`. If it says `systemd`, it's `systemd`.

### How to make the change?

Run `sudo systemctl edit bluetooth.service` in Terminal. You'll see something like this:

```
### Editing /etc/systemd/system/bluetooth.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file



### Edits below this comment will be discarded


### /usr/lib/systemd/system/bluetooth.service
# [Unit]
# Description=Bluetooth service
# Documentation=man:bluetoothd(8)
# ConditionPathIsDirectory=/sys/class/bluetooth
```

Make it look like this:

```
### Editing /etc/systemd/system/bluetooth.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file

[Unit]

Before=graphical.target

### Edits below this comment will be discarded


### /usr/lib/systemd/system/bluetooth.service
# [Unit]
# Description=Bluetooth service
# Documentation=man:bluetoothd(8)
# ConditionPathIsDirectory=/sys/class/bluetooth
```

# AppImage file failing to start - "binary found, misconfigured" error

## Error message

> [9519:0329/191759.367799:FATAL:setuid_sandbox_host.cc(163)] The SUID sandbox helper binary was found, but is not configured correctly. Rather than run without sandboxing I'm aborting now. You need to make sure that /tmp/.mount_tutanoIe1cLD/chrome-sandbox is owned by root and has mode 4755.

## Solution

[Source](https://github.com/electron/electron/issues/42510#issuecomment-2171583086)

> I found why it is failing, and I also found the solution.
> 
> [2.3.2 AppImage fails to start due to missing sandboxing #2429](https://github.com/arduino/arduino-ide/issues/2429#issuecomment-2099775010)
> 
> As mentioned in the reference documentation, the problem is that Ubuntu 24.04 implemented new restrictions for AppImage apps, which restricts the use of sandboxes.
> 
> The solution is to lift the restrictions that Ubuntu 24.04 implements in the AppImages.
> 
> `sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0`
> 
> for deactivate restrictions
> 
> `sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=1`
> 
> for activate restrictions

# Heroic Launcher needs Steam flatpak

Location is:

    ~/.var/app/com.valvesoftware.Steam/data/Steam

# Radeon Drivers installation

[Source](https://forum.zorin.com/t/problem-with-amdgpu-install/15598)

`sudo add-apt-repository ppa:oibaf/graphics-drivers`

`sudo apt update && sudo apt full-upgrade`

# Handling a .deb file

## Installation

`sudo dpkg -i packagename.deb`

## Removal

`sudo dpkg -r packagename.deb`

# Signal - set spellcheck for multiple languages

`flatpak override --user --env=LANGUAGE=en_GB.UTF-8:pl_PL.UTF-8:sv_SE.UTF-8 org.signal.Signal`

# Sticky edge between two screens

System Settings -> Input & Output -> Mouse & Touchpad -> Screen Edges

Change **Edge barrier** to 0 px/None.
