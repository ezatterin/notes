# Logical volumes and stuff

- **pv** - Physical volume
- **vg** - Volume Group
- **lv** - Logical volume

> note: more or less everything below requires sudo.

```bash
pvdisplay
--- Physical volume ---
PV Name               /dev/sda1
VG Name               vg
PV Size               237.97 GiB / not usable 2.00 MiB
Allocatable           yes (but full)
PE Size               4.00 MiB
Total PE              60919
Free PE               0
Allocated PE          60919
PV UUID               1vfjDb-aUEk-I19N-r6pr-1hWH-I4de-wQB3Iu
```

```bash
vgdisplay
--- Volume group ---
VG Name               vg
System ID             
Format                lvm2
Metadata Areas        1
Metadata Sequence No  19
VG Access             read/write
VG Status             resizable
MAX LV                0
Cur LV                5
Open LV               5
Max PV                0
Cur PV                1
Act PV                1
VG Size               237.96 GiB
PE Size               4.00 MiB
Total PE              60919
Alloc PE / Size       60919 / 237.96 GiB
Free  PE / Size       0 / 0   
VG UUID               BO5jbR-Ye78-g7ej-873o-mk1Y-kPZv-HYLYxw
```

```bash
lvdisplay # too long to output
```

The latter shows info on all mounted Logical volumes.

## If you have a brand new HDD

### Create a PV

Find out what disks you have installed:
```bash
fdisk -l
```
You may not have a partition table yet, if the disk is really brand new.
Make one with Gparted of `fdisk` (a different topic in itself). You might
also need to create a physical volume on the created partitions:

```bash
pvcreate /dev/sdb1 # or whatever the partition is called
```

### Create a VG

Easy:
```bash
vgcreate vgtest /dev/sdb1
```

### Create a LV

If it's the first one you make, easy:
```bash
lvcreate -L 50%FREE -n lvstuff vgtest
```
This makes the `lvstuff` LV in the `vgtest` VG with size 50% of what's
available in `vgtest`.

Now you just have a file! To make it a filesystem, you must create one:
```bash
mkfs -t ext4 /dev/vgtest/lvstuff
```

### Mount the LV
Easy:
```bash
mkdir /mtn/stuff
mount -t ext3 /dev/vgtest/lvstuff /mtn/stuff
```
Voila!

## If you have a PV/VG/LV already

This is where it gets kind of messy.

### Add a new HDD to an existing VG

```bash
vgextend vgtest /dev/sdc1
```

### Extend the size of an existing LV
```bash
lvextend -L+5G /dev/vgtest/lvstuff
```
This increases the size of the LV but not of the filesystem it contains!
Do that by:
sudo
```bash
resize2fs /dev/vgtest/lvstuff
```

#### If something weird happens here
Check the state of the fs
```bash
e2fsck /dev/vgtest/lvstuff
```
## Other useful things

Check filesystem of a LV:
```bash
blkid /dev/vgtest/lvstuff
```

Find superblocks backups:

```bash
mke2fs -n /dev/vgtest/lvstuff
```
If `resize2fs` complains about them, restore the backup with
`e2fsck -b <sblock_backup> /dev/vgstest/lvstuff`.

## Some commands to check

- `df -Th` report filesystem disk usage;
- `lsblk` list block devices;
- `cat /etc/fstab` see (or modify) how block devices are mounted.
