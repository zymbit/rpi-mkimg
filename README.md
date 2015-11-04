# mkimg.sh #

This shell script creates a distributable image from a Raspberry Pi SD card.

**NOTE**: This script has **not** been used for imaging irreplaceable data.
While this script *should not* be destructive, it modifies the filesystem and
partition table.

It can be run like this:

```
bash mkimg.sh /dev/sda sdcard.img.zip
```

## What does the script do ##

Under the hood the script performs the following operations:

- ensures the first partition is `fat16` and the second partition `ext4`
- fixes any errors on the filesystem
- shrinks the Linux filesystem to its smallest size
- shrinks the Linux partition to the size of the filesystem + small buffer
- creates a compressed image from the given device
- expands the partition and filesystem back to their largest sizes


## Restoring the filesystem size ##

This script shrinks the Linux volume and partition in order to create a small
image that can be distributed and used to create other cards.  In order to make
all the space on the SD card usable, the filesystem and partition need to be
expanded back to their full size.  For example, the command below would resize
back to fill a 16GB SD card:

```
parted /dev/sda resizepart 16.0GB
resize2fs /dev/sda2
```


## Flash SD card with the image ##

### Linux ###

Replace `<device>` with the location of your SD card; e.g. `/dev/sda`:

```
unzip -p sdcard.img.zip | sudo dd bs=1M of=<device>
```


### Mac ###

Replace `<device>` with the location of your SD card; e.g. `/dev/rdisk1`:

```
unzip -p sdcard.img.zip | sudo dd bs=1m of=<device>
```
