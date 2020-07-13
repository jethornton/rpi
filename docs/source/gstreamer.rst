=========
Gstreamer
=========

To play a mp3 file on the Rpi Gstreamer seems to be the best option.

Install the Gstreamer libraries
::

  sudo apt install libgstreamer1.0-0
  sudo apt install gstreamer-1.0
  sudo apt install gstreamer1.0-tools
  sudo apt install gstreamer1.0-plugins-good
  sudo apt install gstreamer1.0-plugins-bad
  sudo apt autoremove

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

We don't want the bcm2835 ALSA module to be default so disable it in the boot.config.
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

Make a directory and copy a mp3 file into it.
::

  mkdir music

Copy some mp3 files to the music directory, I use FileZilla to copy from PC to
PC on my LAN. So on your PC you use to SSH into the Rpi install FileZilla
::

  sudo apt install filezilla

Now we can test Gstreamer out to make sure everything is ok.
You must use the full path to the mp3 file.
::

  gst-launch-1.0 playbin uri=file:///home/john/music/Alabam.mp3

Note, if the file name has spaces you must enclose the entire path in quotes
like this
::

  gst-launch-1.0 playbin uri=file://"/home/john/music/All Along The Watchtower.mp3"

Install PyQt5 then gi to run the following test program
::

  sudo apt install python3-gi

gst-test.py
::

  #!/usr/bin/python3

  import sys, os, random
  from PyQt5.QtWidgets import (QApplication, QMainWindow, QPushButton,
	  QGridLayout, QWidget)
  from PyQt5.QtGui import QIcon
  import gi
  gi.require_version('Gst', '1.0')
  from gi.repository import Gst

  class Window(QMainWindow):

	  def __init__(self):
		  super().__init__()
		  self.setGeometry(50, 50, 500, 300)
		  self.setWindowTitle("GStreamer Test")
		  self.setWindowIcon(QIcon('/share/pixmaps/openbox.png'))
		  centralWidget = QWidget(self)
		  self.setCentralWidget(centralWidget)
		  grid = QGridLayout()
		  centralWidget.setLayout(grid)
		  self.playBtn = QPushButton('Play')
		  grid.addWidget(self.playBtn, 1,1)
		  self.playBtn.clicked.connect(self.playMusic)
		  self.stopBtn = QPushButton('Stop')
		  grid.addWidget(self.stopBtn, 2,1)
		  self.stopBtn.clicked.connect(self.stopMusic)

		  self.raiseBtn = QPushButton('Raise')
		  grid.addWidget(self.raiseBtn, 0,3)
		  self.raiseBtn.clicked.connect(self.raiseVolume)

		  self.lowerBtn = QPushButton('Lower')
		  grid.addWidget(self.lowerBtn, 1,3)
		  self.lowerBtn.clicked.connect(self.lowerVolume)

		  self.muteBtn = QPushButton('Mute')
		  grid.addWidget(self.muteBtn, 2,3)
		  self.muteBtn.clicked.connect(self.muteVolume)

		  self.exitAppBtn = QPushButton('Exit\nApp')
		  grid.addWidget(self.exitAppBtn, 2,0)
		  self.exitAppBtn.clicked.connect(self.close)

		  Gst.init(None)
		  self.player = Gst.ElementFactory.make("playbin", "player")
		  self.player.connect('about-to-finish', self.on_about_to_finish)
		  self.musicList = []
		  bus = self.player.get_bus()
		  bus.add_signal_watch()
		  bus.connect("message", self.on_message)

		  self.statusBar().showMessage('Ready')

		  self.show()

	  def on_about_to_finish(self, *args):
		  if self.musicList:
			  filepath = os.path.join("/home/john/music", self.musicList.pop(0))
			  self.player.set_property('uri', "file://" + filepath)
			  self.player.set_property('volume', 0.1)
			  self.player.set_state(Gst.State.PLAYING)
			  (f, e) = os.path.splitext(filepath)
			  self.statusBar().showMessage(f)
			  print(f"Volume {self.player.get_property('volume')}")
		  else:
			  self.statusBar().showMessage("No More Songs")



	  def playMusic(self):
		  #print('playing')
		  self.musicList = os.listdir('/home/john/music')
		  random.shuffle(self.musicList)

		  filepath = os.path.join("/home/john/music", self.musicList.pop(0))
		  self.player.set_property("uri", "file://" + filepath)
		  #self.player.set_property('volume', 0.1)
		  self.player.set_state(Gst.State.PLAYING)
		  print(f"Volume {self.player.get_property('volume')}")
		  #print(self.player.get_property('current-uri'))
		  filename = os.path.basename(self.player.get_property('current-uri'))
		  (f, e) = os.path.splitext(filepath)
		  self.statusBar().showMessage(f)
		  self.player.set_property('mute', False)

	  def stopMusic(self):
		  self.player.set_state(Gst.State.NULL)

	  def raiseVolume(self):
		  cv = self.player.get_property('volume')
		  print(f"Current Volume {cv}")
		  nv = round(cv + 0.05, 2)
		  print(f"New Volume {nv}")
		  if nv > 1.0:
			  nv = 1.0
		  self.player.set_property('volume', nv)

	  def lowerVolume(self):
		  cv = self.player.get_property('volume')
		  print(f"Current Volume {cv}")
		  nv = round(cv - 0.05, 2)
		  print(f"New Volume {nv}")
		  if nv < 0.0:
			  nv = 0.0
		  self.player.set_property('volume', nv)

	  def muteVolume(self):
		  if self.player.get_property('mute') == False:
			  self.player.set_property('mute', True)
			  self.muteBtn.setText('Muted')
		  else:
			  self.player.set_property('mute', False)
			  self.muteBtn.setText('Mute')
		  #self.player.set_property('volume', 0.0)
		  print(f"Volume {self.player.get_property('volume')}")


	  def on_message(self, bus, message):
		  #print(message.type)
		  t = message.type
		  if t == Gst.MessageType.EOS:
			  self.player.set_state(Gst.State.NULL)
			  #self.button.set_label("Start")
			  self.statusBar().showMessage('Done')
		  #if t == Gst.MessageType.DURATION_CHANGED:
		  #	print('about to end')

  if __name__ == '__main__':
	  app = QApplication(sys.argv)
	  GUI = Window()
	  sys.exit(app.exec_())

