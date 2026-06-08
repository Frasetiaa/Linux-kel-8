# Linux-kel-8

## Connect Wifi
iwctl

## Cek partisi
lsblk

## Atur partisi
cfdisk /dev/partisi (sda/nvme0n1)

3G EFI System
55G Linux FileSystem

## Enskripsi partisi dengan LUKS
cryptsetup luksFormat /dev/partisi root
cryptsetup luksOpen /dev/partisi root [name LUKS]

## Buat LVM
pvcreate /dev/mapper/[name LUKS]
vgcreate [group name] /dev/mapper/[name LUKS]

## Logical Volume
- lvcreate -L 10G [group name] -n root
- lvcreate -L 10G [group name] -n vars
- lvcreate -L 1G [group name] -n vlog
- lvcreate -L 1G [group name] -n vaud
- lvcreate -L 1G [group name] -n vtmp
- lvcreate -L 10G [group name] -n home
- lvcreate -L 10G [group name] -n container

## Format Logical Volume
- mkfs.ext4 /dev/[group name]/root
- mkfs.ext4 /dev/[group name]/vars
- mkfs.ext4 /dev/[group name]/vlog
- mkfs.ext4 /dev/[group name]/vaud
- mkfs.ext4 /dev/[group name]/vtmp
- mkfs.ext4 /dev/[group name]/home
- mkfs.ext4 /dev/[group name]/container

- mkfs.vfat -F32 /dev/[partisi boot]

## Mount Partisi
### root
mount /dev/[group name]/root /mnt
### Boot
- mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p1 /mnt/boot
### /var
- mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/vars /mnt/var
### /var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vlog /mnt/var/log
### /var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vaud /mnt/var/log/audit
### /var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vtmp /mnt/var/tmp
### /home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/home /mnt/home
### Docker Container Storage
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/container /mnt/var/lib/docker
