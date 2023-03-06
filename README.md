# RockyLinux8-Kickstart-iso
How to generate a bootable ISO for Rocky Linux 8.7 with an integrated kickstart file. 

### Download the ISO from [rockylinux.org](https://rockylinux.org/download)

### Create a temporary directory, then mount the ISO and extract the contents to the temporary directory

### Create a kickstart file and place it on the top directory of the temp foler

### Create this ISO 
```bash
genisoimage -U -r -v -T -J -joliet-long -V "Rocky-8-7-x86_64-dvd" -volset "Rocky-8-7-x86_64-dvd" -A "Rocky-8-7-x86_64-dvd" -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e images/efiboot.img -no-emul-boot -o ../auto-rocky8-7min.iso .
```
#### Description of the individual options and arguments used in the command:

```plaintext
-U: Use a single-byte character set for the ISO file.
-r: Generate a Rock Ridge directory tree for the ISO file.
-v: Verbose output mode.
-T: Generate a fully ISO-9660 compliant file system.
-J: Generate a Joliet file system.
-joliet-long: Generate a Joliet file system that allows long file names.
-V "Rocky-8-7-x86_64-dvd": Set the volume name of the ISO file to "Rocky-8-7-x86_64-dvd".
-volset "Rocky-8-7-x86_64-dvd": Set the volume set name of the ISO file to "Rocky-8-7-x86_64-dvd".
-A "Rocky-8-7-x86_64-dvd": Set the application identifier of the ISO file to "Rocky-8-7-x86_64-dvd".
-b isolinux/isolinux.bin: Set the boot image for the ISO file to "isolinux/isolinux.bin".
-c isolinux/boot.cat: Set the boot catalog for the ISO file to "isolinux/boot.cat".
-no-emul-boot: Specify that the ISO file should not use an emulated boot.
-boot-load-size 4: Set the boot loader size to 4 sectors.
-boot-info-table: Include a bootable El Torito info table in the ISO file.
-eltorito-alt-boot: Include an alternative boot image in the ISO file.
-e images/efiboot.img: Set the alternative boot image for the ISO file to "images/efiboot.img".
-o ../auto-rocky8-7min.iso: Specify the output file name and location for the generated ISO file as "../auto-rocky8-7min.iso".
.: Include all files and directories in the current directory in the ISO file.
```
