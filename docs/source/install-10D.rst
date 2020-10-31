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
want to write down the IP address if your going to use SSH later.

Click on `Next` then set the country, language, time zone and check off
use English language and keyboard the press `Next`.

Skip setting a new password for user `pi` we will replace that slice of
pi later with your user name so just press `Next`.

On the next screen usually you just press `Next`.

You can either setup a wireless network or skip it.

Update the software page press `Next` to update.

After the update click `Restart`

After the Rpi boots back up click on the Raspberry in the upper left
corner then select `Preferences` then `Raspberry Pi Configuration`

* System Tab
   * Set the Hostname you want to use
   * Auto login disabled
   * Network at Boot Wait for network
   * Splash Screen up to you, I turn it off
* 
