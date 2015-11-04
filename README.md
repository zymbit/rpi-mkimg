# mkimg.sh #

This shell script creates an image from a Raspberry Pi SD card.  It can be run like this:

```
bash mkimg.sh /dev/sda sdcard.img.zip
```


## What does the script do ##

Under the hood the script performs the following operations:

- ensures the first partition is FAT and the second partition Linux
- runs e2fsck on the filesystem and prompts to fix any errors found
- resizes the Linux filesystem to its smallest size
- resizes the Linux partition to the size of the filesystem + 200MB
- runs dd to create an image from the given device
- zips the image file


## Expanding the filesystem ##

This script shrinks the Linux volume and partition in order to create a small
image that can be distributed and used to create other cards.  In order to make
all the space on the SD card usable, the filesystem and partition need to be
expanded back to their full size.  This can be done with the following
commands:

```
parted /dev/sda resizepart 16.0GB
resize2fs /dev/sda2
```
