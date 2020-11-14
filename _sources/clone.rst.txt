Backup and Restore
==================

Find the location of the SD card in the file system, first see what is
there then insert the SD card and find the new one.
::

	john@d10cave:~$ df -h
	Filesystem      Size  Used Avail Use% Mounted on
	udev            7.8G     0  7.8G   0% /dev
	tmpfs           1.6G   11M  1.6G   1% /run
	/dev/sdd1       213G  158G   44G  79% /
	tmpfs           7.8G  264M  7.6G   4% /dev/shm
	tmpfs           5.0M  4.0K  5.0M   1% /run/lock
	tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
	tmpfs           1.6G   44K  1.6G   1% /run/user/1000
	/dev/sda1       1.8T  233G  1.5T  14% /media/john/data
	john@d10cave:~$ df -h
	Filesystem      Size  Used Avail Use% Mounted on
	udev            7.8G     0  7.8G   0% /dev
	tmpfs           1.6G   11M  1.6G   1% /run
	/dev/sdd1       213G  158G   44G  79% /
	tmpfs           7.8G  264M  7.6G   4% /dev/shm
	tmpfs           5.0M  4.0K  5.0M   1% /run/lock
	tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
	tmpfs           1.6G   48K  1.6G   1% /run/user/1000
	/dev/sda1       1.8T  233G  1.5T  14% /media/john/data
	/dev/sde2       1.5G  1.1G  275M  80% /media/john/rootfs
	/dev/sde1       253M   51M  202M  21% /media/john/boot

sudo fdisk -l
In my case the SD card is sde so using dd to copy and compress the image
::

	dd conv=sparse if=... of=...
	sudo dd conv=sparse if=/dev/sde of=~/backuptest.img
	sudo dd conv=sparse if=/dev/sde | gzip > image1-`date +%d%m%y`.img.gz

sudo umount /dev/sdb1 /dev/sdb2 /dev/sdb3 /dev/sdb4

