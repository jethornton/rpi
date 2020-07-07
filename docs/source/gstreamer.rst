=========
Gstreamer
=========

To play a mp3 file on the Rpi gstreamer seems to be the best option.

Install the gstreamer libraries
::

  sudo apt install libgstreamer1.0-0
  sudo apt install gstreamer1.0-plugins-base
  sudo apt install gstreamer1.0-plugins-good
  sudo apt install gstreamer1.0-plugins-bad

USB Speakers
------------
Plug in the USB speaker to the Rpi, I used Logitech S150 speakers.

Check ALSA modules
::

  cat /proc/asound/modules

  0 snd_bcm2835
  1 snd_bcm2835
  2 snd_usb_audio
  3 snd_usb_audio

Check sound hardware
::

  cat /proc/asound/cards

  0 [b1             ]: bcm2835_hdmi - bcm2835 HDMI 1
                      bcm2835 HDMI 1
  1 [Headphones     ]: bcm2835_headphonbcm2835 Headphones - bcm2835 Headphones
                      bcm2835 Headphones
  2 [U0x46d0x825    ]: USB-Audio - USB Device 0x46d:0x825
                      USB Device 0x46d:0x825 at usb-3f980000.usb-1.2, high speed
  3 [AUDIO          ]: USB-Audio - USB  AUDIO
                    USB  AUDIO at usb-3f980000.usb-1.1.2, full speed

We don't want to use the bcm2835 ALSA module so disable it in the boot.config.
::

  sudo nano /boot/config.txt

Change to match the following
::

  # Enable audio (loads snd_bcm2835)
  #dtparam=audio=on
  dtparam=audio=off

Ctrl x then y then enter to save then reboot.

Recheck the ALSA modules
::

  cat /proc/asound/modules
  0 snd_usb_audio
  1 snd_usb_audio

Now the USB speakers are the default

Test the speakers are set to default with the built in speaker-test.
::

  speaker-test -c2 -t wav -l2

