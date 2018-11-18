# eight-livecd (RHEL8 Beta livecd)

Using lorax on el8 to create livemedia.

## Motivation

I am using RHEL and derivatives (CentOS) pretty much everywhere at work and at home.

Building from the beta a livecd allows me to test quickly if my hardware is supported.
It lowers the cycle of testing hardware and to be able to report bugs to Red Hat.

The good news : all tools needed are now part of RHEL8 Beta repositories.

As RHEL8 is heavly based on Fedora 28, I reused the kickstart provided by `lorax` in `/usr/share/doc/lorax/`.

## Perequisites

 * RHEL8 Beta installation and a license
 * ~ 10GB free space
 * `yum install lorax`
 * SElinux disabled `setenforce 0`

## Usage

Note: As soon as [mock](https://github.com/rpm-software-management/mock/wiki) will be available for el8 I will update this documentation.

Clone this repository:
```
git clone https://github.com/alphacc/eight-livecd
cd eight-livecd
```

Run as root livemedia-creator to create an ISO:
```
livemedia-creator --ks ks/el8-livemedia.ks --no-virt --resultdir  media/el8 --project RHEL-8-beta-live --make-iso --iso-only --iso-name RHEL-8-beta-live-x86_64.iso --releasever 8 --volid RHEL-8-beta-live --title RHEL-8-beta-live --macboot
```

Alternatively, to create PXE files:
```
livemedia-creator --ks ks/el8-livemedia.ks --no-virt --resultdir  /tmp/rhel8 --project RHEL-8-beta-live --make-pxe-live --releasever 8 --volid RHEL-8-beta-live --title RHEL-8-beta-live
```

## Debugging

`lorax` stores log in the current directory:
```
livemedia.log
program.log
```

Anaconda stores debugging info in `/tmp`:
```
dbus.log
ifcfg.log
storage.log
anaconda.log
hawkey.log
program.log
packaging.log
dnf.cache
```

When used with `livemedia-creator`, `yum` 4 aka `dnf` stores his cache in /dnf.package.cache.

## Common issues

 * Check if selinux is disabled :-)
 * ```2018-11-18 15:34:45,635: Unable to init server: Could not connect: Connection refused```: check if the packages in your kickstart exist in RHEL8 (anaconda.log will give you more details).
 * ```Problem zeroing free blocks of disk_img```: check if your loop devices are ok, after few runs I saw some umounts failing. `umount -f` helped.

## Contributing

Please send a PR if you find bugs and/or improvement.
