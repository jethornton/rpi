================
NGINX Web Server
================

Setting up an NGINX web server on a Raspberry Pi
::

  sudo apt update
  sudo apt dist-upgrade
  sudo apt install nginx
  sudo /etc/init.d/nginx start

Add yourself to the www-data group
::

  sudo usermod -a -G www-data john

Give ownership to all the files and folders in the /var/www/html directory
to the www-data group.
::

  sudo chown -R -f john:www-data /var/www/html

Test the install by opening a browser and if your on the Rpi use
`localhost` as the URL if your on another PC on your LAN type in the
IP address of the Rpi like `192.168.1.130`

Delete the default index page created with Nginx
::

  rm /var/www/html/index.nginx-debian.html

  sudo reboot

