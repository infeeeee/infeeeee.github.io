---
layout: default
categories: ebook-reader
tags: hardware
title: Hacking Kobo Clara HD - Part 2 kernels
---

# Hacking Kobo Clara HD - Part 2: kernels

*I haven't tried this yet, it's just speculations!*

There is a [github issue](https://github.com/XCSoar/XCSoar/issues/676) about usb-otg not working with xcsoar, and I read that on the postamarketOS page, that usb-otg is working over there, and it made me thinking, what if I replace them kernel on the kobo version, and add the kernel from POS.

Some findings:

An old issue about extracting the kernel from a kobo image: [Possibly outdated instructions to replace kernel; Kobo Touch ¡¤ Issue #12 ¡¤ kobolabs/Kobo-Reader ¡¤ GitHub](https://github.com/kobolabs/Kobo-Reader/issues/12)

On this thread user Debiatan links to a dead website:

> I have tried finding the beginning of the uImage in the raw dump of the SD card (as explained in http://buffalo.nas-central.org/wiki/How_to_Extract_an_uImage):

Fortuntely this site was crawled before its shutdown:  https://web.archive.org/web/20180805140706/http://buffalo.nas-central.org/wiki/How_to_Extract_an_uImage

I will copy here the data from the site, as I don't want to open it again, as it's really slow:

> ## About
> 
> This article provides a Linux script which extracts the data of an uImage, so it can be re-used for other compatible devices.
> A script is used instead of separate instructions, so that it can be re-used easily for several different uImages.
> 
> ## Extract an uImage
> 
> According to the [U-Boot header definition](https://web.archive.org/web/20180805140706/http://git.denx.de/cgi-bin/gitweb.cgi?p=u-boot.git;a=blob;f=include/image.h) an uImage begins with the hex byte sequence 27 05 19 56 and the header is 64 bytes long.
> 
> The data is directly following the header and could be in 
> compressed form (gzip or bzip2), but this is untypical for embedded 
> devices.
> It is normally a zImage (=compressed Linux kernel plus decompressor for 
> low memory).
> To get the data only the uImage header has to be removed.
> 
> For ARM devices an additional 8-byte header with the machine type is in front of a zImage.
> In this case the additional 8 bytes have to be removed too.
> 
> ### extract_uImage.sh
> 
> The script also tries to determine if the data is a zImage, otherwise it assumes an Image.
> The script is also available [here](https://web.archive.org/web/20180805140706/http://ftp.maddes.net/u-boot/), but most not be up-to-date all the time.
> 
> ```shell
> #!/bin/sh
> 
> #
> # Copyright (C) 2010 Matthias Buecher (http://www.maddes.net/)
> #
> # This program is free software; you can redistribute it and/or modify
> # it under the terms of the GNU General Public License as published by
> # the Free Software Foundation; either version 2 of the License, or
> # (at your option) any later version.
> # http://www.gnu.org/licenses/gpl-2.0.txt
> #
> # This program is distributed in the hope that it will be useful,
> # but WITHOUT ANY WARRANTY; without even the implied warranty of
> # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
> # GNU General Public License for more details.
> #
> # You should have received a copy of the GNU General Public License
> # along with this program; if not, write to the Free Software
> # Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
> #
> 
> UIMAGE=$1
> 
> # check for uImage magic word
> # http://git.denx.de/cgi-bin/gitweb.cgi?p=u-boot.git;a=blob;f=include/image.h
> echo 'Checking for uImage magic word...'
> MAGIC=`dd if="${UIMAGE}" ibs=4 count=1 | hexdump -v -e '1/1 "%02X"'`
> [ '27051956' != "${MAGIC}" ]  && { echo 'Not an uImage.' ; exit 1 ; }
> 
> # extract data from uImage
> echo 'uImage recognized.'
> echo 'Extracting data...'
> DATAFILE='uImage.data'
> dd if="${UIMAGE}" of="${DATAFILE}" ibs=64 skip=1
> 
> # check for ARM mach type ( xx 1C A0 E3 xx 10 81 E3 )
> # http://www.simtec.co.uk/products/SWLINUX/files/booting_article.html#d0e600
> echo 'Checking for ARM mach-type...'
> MAGIC=`dd if="${DATAFILE}" ibs=1 skip=1 count=3 | hexdump -v -e '1/1 "%02X"'`
> [ '1CA0E3' = "${MAGIC}" ] && {
> 	MAGIC=`dd if="${DATAFILE}" ibs=1 skip=5 count=3 | hexdump -v -e '1/1 "%02X"'`
> 	[ '1081E3' = "${MAGIC}" ] && {
> 		echo 'ARM mach-type header recognized.'
> 		echo 'Extracting mach-type header...'
> 		dd if="${DATAFILE}" of="uImage.mach-type" ibs=8 count=1
>                 ARCH=$(hexdump -v -e '1/1 "%02X "' uImage.mach-type); echo "The mach-type is: $ARCH"
> 		echo 'Stripping mach-type header...'
> 		TMPFILE='uImage.tmp'
> 		dd if="${DATAFILE}" of="${TMPFILE}" ibs=8 skip=1
> 		rm -f "${DATAFILE}"
> 		mv "${TMPFILE}" "${DATAFILE}"
> 	}
> }
> 
> # check for zImage, otherwise assume Image
> # http://www.simtec.co.uk/products/SWLINUX/files/booting_article.html#d0e309
> TMPFILE='Image'
> echo 'Checking for zImage...'
> MAGIC=`dd if="${DATAFILE}" ibs=4 skip=9 count=1 | hexdump -v -e '1/1 "%02X"'`
> [ '18286F01' = "${MAGIC}" ] && {
> 	START=`dd if="${DATAFILE}" ibs=4 skip=10 count=1 | hexdump -v -e '1/4 "%08X"'`
> 	END=`dd if="${DATAFILE}" ibs=4 skip=11 count=1 | hexdump -v -e '1/4 "%08X"'`
> #
> 	SIZE=$(( 0x${END} - 0x${START} ))
> #
> 	echo "zImage recognized with start 0x${START}, end 0x${END} and size ${SIZE}."
> 	TMPFILE='zImage'
> }
> mv "${DATAFILE}" "${TMPFILE}" 
> 
> echo ">>> ${UIMAGE} extracted to ${TMPFILE}"
> ```

Fortuntely, the site linked here is still up, the script downloadable from the author:

https://www.maddes.net/files/u-boot/extract_uImage.sh

## TODO

- [ ] Open up kobo

- [ ] Make a copy of the sd card

- [ ] Try to run POS

- [ ] Check if the original image is still bootable

- [ ] Install Xcsoar

- [ ] Export the uImage or zImage from the installed POS 

- [ ] Add the zImage to the kobo image

- [ ] Hope that it boots