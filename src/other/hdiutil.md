# `hdiutil`工具

`hdiutil`是`macOS`下的一个工具，用于管理磁盘镜像。可以操作磁盘映像(附加、验证、刻录等)

支持以下文件系统类型:

| 文件系统                        | 说明                                        |
| ------------------------------- | ------------------------------------------- |
| `UDF`                           | Universal Disk Format (UDF)                 |
| `MS-DOS FAT12`                  | MS-DOS (FAT12)                              |
| `MS-DOS`                        | MS-DOS (FAT)                                |
| `MS-DOS FAT16`                  | MS-DOS (FAT16)                              |
| `MS-DOS FAT32`                  | MS-DOS (FAT32)                              |
| `HFS+`                          | Mac OS Extended                             |
| `Case-sensitive HFS+`           | Mac OS Extended (Case-sensitive)            |
| `Case-sensitive Journaled HFS+` | Mac OS Extended (Case-sensitive, Journaled) |
| `Journaled HFS+`                | Mac OS Extended (Journaled)                 |
| `ExFAT`                         | ExFAT                                       |
| `Case-sensitive APFS`           | APFS (Case-sensitive)                       |
| `APFS`                          | APFS                                        |


```shell
$ hdiutil help
Usage: hdiutil <verb> <options>
<verb> is one of the following:
help            	imageinfo
attach          	isencrypted
detach          	makehybrid
eject           	mount
verify          	mountvol
create          	unmount
compact         	plugins
convert         	resize
burn            	segment
info            	pmap
checksum        	udifderez
chpass          	udifrez
erasekeys       	
help			   display more detailed help

Usage:	hdiutil attach [options] <image>
	hdiutil attach -help

Usage:	hdiutil detach [options] <devname>
	hdiutil detach -help

	eject is a synonym for detach...
Usage:	hdiutil detach [options] <devname>
	hdiutil detach -help

Usage:	hdiutil verify [options] <image>
	hdiutil verify -help

Usage:	hdiutil create <sizespec> [options] <imagepath>
	hdiutil create -help

Usage:	hdiutil compact [options] <image>
	hdiutil compact -help

Usage:	hdiutil convert -format <format> -o <outfile> [options] <image>
	hdiutil convert -help

Usage:	hdiutil burn [options] <image>
	hdiutil burn -help

Usage:	hdiutil info [options]
	hdiutil info -help

Usage:	hdiutil checksum -type <checksumType> [options] <image>
	hdiutil checksum -help

Usage:	hdiutil chpass [options] <image>
	hdiutil chpass -help

Usage:	hdiutil erasekeys <image>
	hdiutil erasekeys -help

Usage:	hdiutil imageinfo [options] <image>
	hdiutil imageinfo -help

Usage:	hdiutil isencrypted <image>
	hdiutil isencrypted -help

Usage:	hdiutil makehybrid -o <outfile> [options] <source>
	hdiutil makehybrid -help

Usage:	hdiutil attach [options] <image>
	hdiutil attach -help

Usage:	hdiutil mountvol [options] <devname>
	hdiutil mountvol -help

Usage:	hdiutil unmount [options] <mountpoint>
	hdiutil unmount -help

Usage:	hdiutil plugins [options]
	hdiutil plugins -help

Usage:	hdiutil resize <sizespec> [options] <image>
	hdiutil resize -help

Usage:	hdiutil segment -o <outfile> -segmentCount <num> [options] <image> (deprecated)
	hdiutil segment -o <outfile> -segmentSize <size> [options] <image> (deprecated)
	hdiutil segment -help

Usage: hdiutil pmap [options] <image|device>
	hdiutil pmap -help

Usage:	hdiutil udifderez [options] <image>
	hdiutil udifderez -help

Usage:	hdiutil udifrez [options] <image>
	hdiutil udifrez -help
```