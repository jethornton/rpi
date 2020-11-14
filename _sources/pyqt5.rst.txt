=====
PyQt5
=====

PyQt5 is a python port of QT.

Install in a terminal
::

  sudo apt install python3-pyqt5

To test the installation create this simple program in your bin directory
::

  nano ~/bin/simple.py

simple.py
::

  #!/usr/bin/python3

  import sys
  from PyQt5.QtWidgets import QApplication, QWidget

  def main():
      app = QApplication(sys.argv)

      w = QWidget()
      w.resize(250, 150)
      w.move(300, 300)
      w.setWindowTitle('Simple')
      w.show()

      sys.exit(app.exec_())


  if __name__ == '__main__':
      main()

Now we must make the file executable by setting the 
'file permissions <https://www.washington.edu/computing/unix/permissions.html>_`
with chmod. Note the ~ is short hand for the users home directory.
::

  chmod u=rwx ~/bin/simple.py

Now we can test that the file permissions have been set with the ls command.
::

  ls -lg ~/bin/simple.py
  -rwxr--r-- 1 pi 301 Jul 10 07:36 /home/john/bin/simple.py


To execut the file you must do that on the Rpi. Right click and select Terminal
Emulator.
::

  simple.py
