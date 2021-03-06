
Configuration and Installation
===================
Mac OS X is picky about hardware, so there are a few settings that must be
set in VirtualBox in order to make it run:
  - Firmware type: BIOS
  - Number of CPUs: 1

Mount the EmpireEFI4AMD.iso in your virtual machine, and start the VM
headlessly, then connect over RDP.

You will see the EmpireEFI bootloader splash screen. Unmount the EmpireEFI
ISO and mount the Mac OS X install DMG as a disc. 

Go back to your RDP session and hit F5. This should cause EmpireEFI to rescan
the DVD drive and the Mac OS X Install disc should appear. Select that and
hit enter.

You may need to partition the hard disk before continuing with the install. 
This can be done by selecting "Disk Utility" from the menu at the top of the 
installation screen. 

Proceed with the Mac OS X installation as normal. At the end of the
installation, it may report that the disk could not be made bootable, but this
is safe to ignore.

Mount the EmpireEFI disc again and reboot. When the EmpireEFI bootloader 
screen appears, select Macintosh HD to boot.

**Warning**:
In order to boot without the EmpireEFI boot disc, run the myHack installer
located on the boot disc. (Run this while you're in Mac OS X.) Unfortunately,
this broke the VM when I tried it, so make sure you snapshot before you try it.
It worked fine for me when the host machine had an Intel processor though.

You should be able to make it through the user creation and end up at the
Mac OS X desktop. To speed up RDP sessions, change the background to a flat
colour. 

You should also disable everything in the Energy Saver preferences pane to 
prevent hard drive spindowns and suspends, and disable the screensaver
because it eats CPU like crazy without proper video acceleration. 
Alternatively, you can change these settings from the terminal using the 
commands below.

To disable the screensaver from the terminal:
```  
$ defaults -currentHost write com.apple.screensaver idleTime 0
```

To disable sleeping of peripherals and the VM itself from the terminal, run:
```
$ sudo pmset disksleep 0 displaysleep 0 sleep 0
```

Next, you should enable Remote Login (SSH) in System Preferences. You can 
set up SSH keys now too, if you'd like.

When Hudson or Seamstress tries to login remotely via SSH, it will require
the PATH to be set up to something sane. Edit `/etc/sshd_config` and enable
```
PermitUserEnvironment yes
```

Next, create or edit ~/.ssh/environment and add:
```
PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/local/bin:/opt/bin
```

When Hudson tries to launch builds via SSH, it should now find bzr and scons
successfully.

Pro Tips
===================

To mount a .dmg from the command-line easily, run:
```  
$ open [CoolSoftware.dmg]
```

To unmount it, you can run:
```
$ diskutil eject /Volumes/CoolSoftware
```

You can install a .pkg or .mpkg from the terminal without a GUI using the
installer command like so:
```
$ sudo installer -pkg Bazaar-2.3b4-1.mpkg -target /
```
