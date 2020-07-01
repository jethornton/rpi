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


