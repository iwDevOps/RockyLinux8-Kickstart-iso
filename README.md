# RockyLinux8-Kickstart-iso
How to make an ISO image with an integrated kickstart file from RockyLinux 8 
```bash
# Download RockyLinux
curl -# -O https://download.rockylinux.org/pub/rocky/8/isos/x86_64/Rocky-8.7-x86_64-dvd1.iso
```
```bash
# Mount iso, then copy all contents 
mkdir -p /mnt/iso /var/ftp/pub/Rocky8-7_ISO
mount Rocky-8.7-x86_64-dvd1.iso /mnt/iso
cp -prv /mnt/iso/* /var/ftp/pub/Rocky8-7_ISO
umount /mnt/iso
```

Verify. The .treeinfo and .discinfo files are imporant. Make sure the are present
```bash
cd /var/ftp/pub/Rocky8-7_ISO
```
```bash

# Verify with ls -la
ls -la
```
```bash
total 64
drwxr-xr-x. 8 root root   222 Mar  4 17:01 .
drwxr-xr-x. 4 root root    93 Mar  4 17:14 ..
dr-xr-xr-x. 4 root root    38 Nov 10 15:36 AppStream
dr-xr-xr-x. 4 root root    38 Nov 10 15:36 BaseOS
drwxr-xr-x. 3 root root    17 Mar  4 14:33 boot
-r--r--r--. 1 root root    43 Nov 10 15:34 .discinfo
dr-xr-xr-x. 3 root root    18 Nov 10 15:36 EFI
dr-xr-xr-x. 3 root root    76 Nov 10 15:36 images
dr-xr-xr-x. 2 root root   256 Mar  4 17:01 isolinux
-rw-------. 1 root root  1623 Mar  4 15:07 ks.cfg # Kickstart file
-r--r--r--. 1 root root  2204 Oct 18 19:48 LICENSE
-r--r--r--. 1 root root    86 Nov 10 15:34 media.repo
-rw-r--r--. 1 root root   142 Feb 24 19:09 menu.cfg
-rw-r--r--. 1 root root 34816 Feb 24 19:26 output.iso
-r--r--r--. 1 root root   883 Nov 10 15:36 TRANS.TBL
-r--r--r--. 1 root root  1516 Nov 10 15:34 .treeinfo
```
Add these three files to the corresponding directories. Except for the ks.cfg, the other files are present, so delete them and add these. 
1. Add the kickstart file


```bash
 tree -P "ks.cfg|isolinux.cfg|grub.cfg" --prune
.
└── Rocky8-7_ISO
    ├── EFI
    │   └── BOOT
    │       └── grub.cfg
    ├── isolinux
    │   └── isolinux.cfg
    └── ks.cfg  # kickstart file
```

From inside the Rocky8-7_ISO directory, make the iso with this command:

`NOTE:`The new iso is written to /var/ftp/pub/auto-iwa-rocky8-7.iso
```bash
genisoimage -U -r -v -T -J -joliet-long -V "Rocky-8-7-x86_64-dvd" -volset "Rocky-8-7-x86_64-dvd" -A "Rocky-8-7-x86_64-dvd" -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e images/efiboot.img -no-emul-boot -o /var/ftp/pub/auto-iwa-rocky8-7.iso
```

Description of the individual options and arguments used in the command:

```plaintext
-U: Use a single-byte character set for the ISO file.
-r: Generate a Rock Ridge directory tree for the ISO file.
-v: Verbose output mode.
-T: Generate a fully ISO-9660 compliant file system.
-J: Generate a Joliet file system.
-joliet-long: Generate a Joliet file system that allows long file names.
-V: Set the volume name of the ISO file to "Rocky-8-7-x86_64-dvd".
-volset: Set the volume set name of the ISO file to "Rocky-8-7-x86_64-dvd".
-A: Set the application identifier of the ISO file to "Rocky-8-7-x86_64-dvd".
-b: Set the boot image for the ISO file to "isolinux/isolinux.bin".
-c: Set the boot catalog for the ISO file to "isolinux/boot.cat".
-no-emul-boot: Specify that the ISO file should not use an emulated boot.
-boot-load-size 4: Set the boot loader size to 4 sectors.
-boot-info-table: Include a bootable El Torito info table in the ISO file.
-eltorito-alt-boot: Include an alternative boot image in the ISO file.
-e: Set the alternative boot image for the ISO file to "images/efiboot.img".
-o: Specify the output file name and location for the generated ISO file as "../auto-rocky8-7min.iso".
.: Include all files and directories in the current directory in the ISO file.
```
