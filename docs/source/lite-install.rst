====================
Raspberry Pi OS Lite
====================

To get a minimal install of Raspberry Pi OS download the 
`Raspberry Pi OS (32-bit) Lite <https://www.raspberrypi.org/downloads/raspberry-pi-os/>`_
image.

Burn the image to the SD card with `balenaEtcher <https://www.balena.io/etcher/>`_

Insert the SD card and connect a mouse, keyboard and monitor to the Raspberry Pi
and boot up. After the screen stops scrolling you will need to log in. The user
name is `pi` and the password is `raspberry`.

Setting up the Raspberry Pi OS
------------------------------

First some settings need to be changed using raspi-config so type in
::

  sudo raspi-config

Use the Up/Down arrow keys to move in a menu and the tab key to move between
menus and the spacebar to select options. The following are for the US, adjust
as needed for your country.

1 Localisation Options
  * Change Locale to `en_US ISO8859-1` tab to Ok and press Enter
  * Default Locale `en_US` tab to Ok and press Enter

2 Time Zone select US then your time zone.

3 Change WLAN Country to your US

4 Interfacing Options Enable the following
  * SSH
  * 1-Wire

4 Advanced Options
  * Expand File System

Finish and Reboot

Setup a New User
----------------

If you have another PC connected to a LAN you can now work from that PC via SSH.
If not then you will have to do this on the Rpi.

Log in as user pi with the password raspberry
::

  ssh pi@192.168.x.xxx replace x's with your Rpi's address
  sudo adduser pia

set the password and press enter until you get the is this correct then y

Allow the new user to run sudo by adding the user to sudo group:
::

  sudo adduser pia sudo

Reboot the Rpi
::

  sudo reboot

On the Rpi log in as pia with your password



Install OpenBox
---------------

Update the repositories
::

  sudo apt update

Install Xorg and Xinit
::

  sudo apt install --no-install-recommends xserver-xorg xinit

Install Openbox LXTerminal LightDM
::

  sudo apt install openbox lxterminal lightdm


