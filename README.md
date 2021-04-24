
Here is set of my autoinst.xml scripts to make
unattended install of openSUSE LEAP 15.2

General requirements:
- having `virt-manager` installed and working

Following Autoinst scripts are available:

## virt-minimal

Location: `virt-minimal/autoinst.xml`

Minimal and offline installation of `openSUSE LEAP 15.2`.

Requirements:
- create virtual `floppy.img` with `autoinst.xml` in root
  directory from `virt-minimal/autoinst.xml`. Please
  see my guide https://github.com/hpaluch/hpaluch.github.io/wiki/Linux-guest-under-KVM#create-virtual-floppy
- download Full `openSUSE-Leap-15.2-DVD-x86_64.iso`, for example from:
  - http://ftp.linux.cz/pub/linux/opensuse/distribution/leap/15.2/iso/openSUSE-Leap-15.2-DVD-x86_64.iso
  - WARNING! Do NOT use NET version of DVD. This installation requires
    above full DVD to work properly, because it is fully offline.

Performing installation (hopefully I will later use `virt-inst` someday):
- run `virt-manager`
- click on `Create a new virtual machine`
- select 1st option `Local install media ...`, Forward
- select your `openSUSE-Leap-15.2-DVD-x86_64.iso`
  ( click on `Browse...` -> `Browse Local` and select above ISO), Forward
- ensure that `opensSUSE` appears in `Choose the operating system...`, Forward  
- set Memory (minimum: `1024` MB) and CPU (minimum: `1` ), Forward
- keep `Create a disk image...` selected, change size to `16.0` GB or higher,
  Forward
- optional: change `Name:` to any suitable name
- check checkbox `Customize configuration before install`, Forward
- click on Finish

Now we need to make these changes to VM configuration:
- Optional: remove `USB Redirector 2` and `USB Redirector 1` to
  avoid grabbing USB device inserted to your Host PC.
- Optional: remove `Sound ich9`
- Optional: set aggressive disk caching to speedup installation:
  - click on `VirtIO Disk 1`
  - select `Advanced optons` -> `Performance options` `Cache mode:` set
    to `unsafe`
- Required: add your `floppy.img` to this VM:
  - click on `Add Hardware`
  - there should be already selected `Storage`
  - select `Floppy Device` in `Device Type:`
  - click on `Manage` -> `Browse Local` and select your 
    `floppy.img` that must contain this `autoinst.xml`,
    click on `Finish`
  - Optional:  select `Floppy 1` device and click on `Readonly`
    to avoid accident change from VM

Now click on `Begin installation`

When installer appears do this:
- select `Installation` from menu
- write to Boot Options:
  -  `autoyast=device://fd0/autoinst.xml`
  - NOTE: because the 1st installation stage is now fully offline
    we don't need to append `ifcfg=eth0=dhcp` to have network in 1st stage
    (NOTE: YaST2 will configure our final network later - after reboot)
- Finally press ENTER to begin installation.

Optional: in `virt-manager` I prefer to set `View`->
`Scale display` -> `Always` to stand ridiculously high resolution
set by KMS while installing openSUSE (thoretically you cand override
it by appending `video=Virtual-1:1024x768` to `Boot Options:` but I'm to lazy :-)




  


