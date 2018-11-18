# eight-livecd (RHEL8 Beta livecd)

Using lorax on el8 to create livemedia.

## Motivation

I am using RHEL and derivatives (CentOS) pretty much everywhere at work and at home.

Building from the beta a livecd allows me to test quickly if my hardware is supported.
It lowers the cycle of testing hardware and to be able to report bugs to Red Hat.

The good news : all tools needed are now part of RHEL8 Beta repositories.

As RHEL8 is heavly based on Fedora 28, I reused the kickstart provided by `lorax` in `/usr/share/doc/lorax/`.
A simple `diff` will show you what I updated/changed.

## Status (WIP)

| Version | kickstart name | Status ISO | Status PXE |
| ------- | -------------- | ---------- | ---------- |
| minimal | el8-livemedia-minimal.ks | OK | OK |
| workstation | el8-livemedia.ks | FAIL | FAIL |

## Perequisites

 * RHEL8 Beta installation and a license
 * ~ 10GB free space
 * `yum install lorax`
 * SElinux disabled `setenforce 0`

## Usage

*Warning*: using --novirt could crash your local install (from lorax documentation```It may totally trash your host. So far I haven’t had this happen, but the possibility exists that a bug in Anaconda could result in it operating on real devices. I recommend running it in a virt or on a system that you can afford to lose all data from.```)

As soon as [mock](https://github.com/rpm-software-management/mock/wiki) will be available for el8 I will update this documentation.

Now let's start:

Clone this repository:
```
git clone https://github.com/alphacc/eight-livecd
cd eight-livecd
```

Run as root livemedia-creator to create an ISO:
```
livemedia-creator --ks ks/el8-livemedia-minimal.ks --no-virt --resultdir  media/el8 --project RHEL-8-beta-live --make-iso --iso-only --iso-name RHEL-8-beta-live-x86_64.iso --releasever 8 --volid RHEL-8-beta-live --title RHEL-beta-live --macboot
```

Alternatively, to create PXE files:
```
livemedia-creator --ks ks/el8-livemedia-minimal.ks --no-virt --resultdir  media/el8 --project RHEL-8-beta-live --make-pxe-live --releasever 8 --volid RHEL-8-beta-live --title RHEL-beta-live
```

## Debugging

`lorax` stores log in the current directory:
```
livemedia.log
program.log
```

Anaconda stores debugging info in `/tmp` and `lorax` copies it in `./anaconda` in your current directory:
```
dbus.log
ifcfg.log
storage.log
anaconda.log
hawkey.log
program.log
packaging.log
dnf.cache
dnf.librepo.log
```

When used with `livemedia-creator`, `yum` 4 aka `dnf` stores his cache in /dnf.package.cache.

## Common issues

 * Check if selinux is disabled `setenforce 0` :-)
 * ```2018-11-18 15:34:45,635: Unable to init server: Could not connect: Connection refused```: check if the packages in your kickstart exist in RHEL8 (anaconda.log will give you more details).
 * ```Problem zeroing free blocks of disk_img```: check if your loop devices are ok, after few runs I saw some umounts failing. `umount -f` helped. However I had to reboot the VM once.

## Contributing

Please send a PR if you find bugs and/or improvement.
