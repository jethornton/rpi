==========================
Raspberry Pi OS 10 Desktop
==========================

This tutorial is done using 2020-08-20-raspios-buster-armf.zip or better
known as Raspberry Pi OS (32-bit) with desktop from 
`here <https://www.raspberrypi.org/downloads/raspberry-pi-os/>`_

Flash the image to the SD card with either
`balenaEtcher <https://www.balena.io/etcher/>`_ or use the
`Raspberry Pi Imager <https://www.raspberrypi.org/downloads/>`_ for the
OS that you have on your PC.

Connect a mouse, keyboard and monitor to the Raspberry Pi and boot up.

When you boot up to the desktop you get the welcome screen, you might
want to write down the IP address if your going to use SSH or VNC later.

.. image:: images/welcome-01.png
   :align: center

Click on `Next` then set the country, language, time zone and check off
use English language and keyboard the press `Next`.

.. image:: images/welcome-02.png
   :align: center

Set the password you want to use and press `Next`.

.. image:: images/welcome-03.png
   :align: center

On the next screen usually you just press `Next`.

.. image:: images/welcome-04.png
   :align: center

You can either setup a wireless network or skip it.

.. image:: images/welcome-05.png
   :align: center

Update the software page press `Next` to update.

.. image:: images/welcome-06.png
   :align: center

After the update click `Restart`

.. image:: images/welcome-07.png
   :align: center

After the Rpi boots back up click on the Raspberry in the upper left
corner then select `Preferences` then `Raspberry Pi Configuration`

* System Tab
   * Set the Hostname you want to use
   * Network at Boot Wait for network
   * Splash Screen up to you, I turn it off
* Interfaces
   * SSH Enable
   * VNC up to you

Click `Ok` and reboot

You can continue on the Rpi in a terminal or log in with SSH from either
a Linux PC in a terminal or a Windoze PC using PuTTy. On the Rpi the
terminal is the black square thing in the upper left corner so click on
that and do the following to change the user name and password. First
add a root password of your choice but don't forget it.
::

  sudo passwd root

Set the password then reboot and login as root using the password you
set.

Change the user name with `usermod -l newname pi` I'll use john as the
newname
::

  usermod -l john pi

Reboot and login as the new user

You can now setup auto login in `Raspberry Pi Configuration`, don't
it says auto login for pi it also says auto login for default user which
is you now.


