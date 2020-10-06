====================
Raspberry Pi OS Lite
====================

To get a minimal install of Raspberry Pi OS download the 
`Raspberry Pi OS (32-bit) Lite <https://downloads.raspberrypi.org/raspios_lite_armhf_latest>`_
image.

Burn the image to the SD card with `balenaEtcher <https://www.balena.io/etcher/>`_


If your using a Debian based OS you can install and have it on the menu.

* Add Etcher debian repository

::

  echo "deb https://deb.etcher.io stable etcher" | sudo tee /etc/apt/sources.list.d/balena-etcher.list

* Trust Bintray.com's GPG key

::

  sudo apt-key adv --keyserver hkps://keyserver.ubuntu.com:443 --recv-keys 379CE192D401AB61

* Update and install

::

  sudo apt-get update
  sudo apt-get install balena-etcher-electron

* Burn the image to the SD card with Etcher

Insert the SD card and connect a mouse, keyboard and monitor to the Raspberry Pi
and boot up. After the screen stops scrolling you will need to log in. The user
name is `pi` and the password is `raspberry`. At this point you have an
Operating System (OS) but nothing else.

Setting up the Raspberry Pi OS
------------------------------

First some settings need to be changed using raspi-config. After booting
up open a terminal and type in the following.
::

  sudo raspi-config

Use the Up/Down arrow keys to move in a menu and the tab key to move between
menus and the spacebar to select options. The following are for the US, adjust
as needed for your country.

2 Network Options
  * Hostname set the name you want to be visible on the LAN

1 Localisation Options
  * Change Locale to `en_US ISO8859-1` tab to Ok and press Enter
  * Default Locale `en_US` tab to Ok and press Enter

  * Time Zone, Select US then your time zone.

  * Change WLAN Country to US

4 Interfacing Options Enable the following
  * SSH
  * 1-Wire

4 Advanced Options
  * Expand File System

Finish and Reboot

Setup the User
--------------

At the Rpi log in as user pi with the password raspberry

If you have another PC connected to a LAN you can now work from that PC via SSH.
At the Rpi type in `hostname -I` to find out the IP address of the Rpi.

To SSH from another Linux PC on the LAN
::

  ssh pi@192.168.n.nnn replace n's with your Rpi's address

If not using SSH then you will have to do this on the Rpi.
::

  sudo adduser pia

set the password and press enter until you get the is this correct then y

Allow the new user to run sudo by adding the user to sudo group:
::

  sudo adduser pia sudo

Reboot the Rpi
::

  sudo reboot

On the Rpi log in as pia with your password

If your doing this from another PC on the LAN ssh back in as pia
::

  ssh pia@192.168.n.nnn

Change the user name of pi to your user name in my case it's john
sudo usermod -l newUsername oldUsername
::

  sudo usermod -l john pi

Change the home directory name to your name again for me it's john

sudo usermod -d /home/newHomeDir -m newUsername
::

  sudo usermod -d /home/john -m john

Update the password to your favorite password
::

  sudo passwd john

Reboot the Rpi
::

  sudo reboot

On the Rpi log in as your new user name

If your doing this from another Linux PC with the same user name ssh
back in as you by just using the IP address
::

  ssh 192.168.n.nnn

Delete temporary user and folder
::

  sudo deluser pia
  sudo rm -r /home/pia

Update everything
::

  sudo apt update
  sudo apt dist-upgrade
  sudo apt clean

At this point we have an up to date OS with nothing else.

Static IP Address
-----------------

If you want to have the same IP address on the Rpi

Find the IP of the router with
::

  ip r | grep default
  default via 192.168.1.1 dev enp5s0 proto dhcp metric 100 

Now edit dhcpcd.conf
::

  sudo nano /etc/dhcpcd.conf

Change the following lines to the address you want and remove the #
::

  # Example static IP configuration:
  #interface eth0
  #static ip_address=192.168.0.10/24
  #static ip6_address=fd51:42f8:caae:d92e::ff/64
  #static routers=192.168.0.1
  #static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1

  # Example static IP configuration:
  interface eth0
  static ip_address=192.168.1.135/24
  #static ip6_address=fd51:42f8:caae:d92e::ff/64
  static routers=192.168.1.1
  #static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1

Ctrl x then y then enter to save. Reboot to apply and log back in at the Rpi.

User bin Directory
------------------

To add a bin directory and make .bashrc add that to the path so any
executables you place in the /home/username/bin will run from the
command line or as a program you need to edit the /home/username/.bashrc
file. From the users home directory open a terminal and do the following.
::

	ls -a

If bin is not there add it
::

	mkdir bin
	nano ~/.bashrc

Add the following to the end of the file
::

	# set PATH so it includes user's private bin if it exists
	if [ -d "$HOME/bin" ] ; then
			PATH="$HOME/bin:$PATH"
	fi

	# set PATH so it includes user's private bin if it exists
	if [ -d "$HOME/.local/bin" ] ; then
			PATH="$HOME/.local/bin:$PATH"
	fi

Press Ctrl X then y then enter to save the changes

Install OpenBox
---------------

From either a SSH connection or on the Rpi.

Install Xorg, Xinit and X11 Utilities
::

  sudo apt install --no-install-recommends xserver-xorg xinit x11-xserver-utils

Install Openbox LXTerminal LightDM
::

  sudo apt install openbox lxterminal lightdm

Setup auto login
::

  sudo nano /etc/lightdm/lightdm.conf

Scroll down to the section [Seat:\*] and change these two lines
::

  #autologin-user=
  #autologin-user-timeout=0

  autologin-user=your user name
  autologin-user-timeout=0

Ctrl x then y then enter to save

Install the OpenBox menu configuration tool which must be ran on the Rpi4 and not from SSH
::

  sudo apt install obmenu

Remove any unused packages with
::

  sudo apt update
  sudo apt autoremove
  sudo apt clean

While we are cleaning up lets delete all the empty directories with
::

  find . -type d -empty -delete

Finally reboot and the Rpi should log you in automaticly.
::

  sudo reboot

After the reboot you will be at a completly blank screen if your logged in.

Right click in the Rpi to open a terminal and test that you have the path set
to include your bin directory. Look for /home/your name/bin in the path
::

  echo $PATH
  /home/john/bin:/usr/local/sbin:... lots of paths

Right click and the menu pops up. Press Ctrl + Alt + Right or Left Arrow
keys to switch desktops. Alt Tab to switch between running programs.

Start a GUI program at bootup
-----------------------------

Add an `autostart` file
::

	sudo nano /etc/xdg/openbox/autostart

Add the full path of the program followed by a space and an ampersand
::

  /home/john/bin/coop &

Ctrl x the y then enter to save

Reboot and your program should start at boot up.

Disable DPMS Screen Blanking
----------------------------

To completely disable DPMS X11 screen blanking, add the following to a
file in /etc/X11/xorg.conf.d/10-monitor.conf

First check to see if the directory `/etc/X11/xorg.conf.d` exists with
::

	ls /etc/X11

If xorg.conf.d is not there create it with
::

	sudo mkdir /etc/X11/xorg.conf.d

Now create the file 10-monitor.conf
::

	sudo nano /etc/X11/xorg.conf.d/10-monitor.conf

Add the following
::

	Section "ServerFlags"
			Option "BlankTime" "0"
			Option "StandbyTime" "0"
			Option "SuspendTime" "0"
			Option "OffTime" "0"
			Option "NoPM" "true"
	EndSection

Ctrl x then y then enter to save the file
Reboot

Disable 
::

	xset s off
	xset s noblank
	xset dpms 0 0 0
	xset -dpms

Check whether the screen blanking has been disabled with this command on
the Rpi not via SSH:
::

	xset q

Auto Login
----------

To automaticly log in to the console
::

	sudo raspi-config

Select "Boot Options".
Select "Desktop/CLI"
Select "Console Autologin"
