+++
archetype = "chapter"
title = "Linux"
weight = 2
+++

### Extend partition

```
fdisk /dev/sda
```

Enter p to print your initial partition table.

Enter d (delete) followed by 2 to delete the existing partition definition (partition 1 is usually /boot and partition 2 is usually the root partition).

Enter n (new) followed by p (primary) followed by 2 to re-create partition number 2 and enter to accept the start block and enter again to accept the end block which is defaulted to the end of the disk.

Enter t (type) then 2 then 8e to change the new partition type to "Linux LVM".

Enter p to print your new partition table and make sure the start block matches what was in the initial partition table printed above.

Enter w to write the partition table to disk. You will see an error about Device or resource busy which you can ignore.

### Update kernel in-memory partition table

After changing your partition table, run the following command to update the kernel in-memory partition table:

```
partx -u /dev/sda
```

### Resize physical volume

Resize the PV to recognize the extra space

```
pvresize /dev/vda2
```

### Resize LV and filesystem

In this command centos is the PV, root is the LV and /dev/vda2 is the partition that was extended. Use pvs and lvs commands to see your physical and logical volume names if you don't know them. The -r option in this command resizes the filesystem appropriately so you don't have to call resize2fs or xfs_growfs separately.

```
lvextend -r centos/root /dev/sda2
```