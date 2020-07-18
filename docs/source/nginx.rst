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

  add john to the www-data group
  sudo usermod -a -G www-data john

Give ownership to all the files and folders in the /var/www/html directory
to the www-data group.
::

  sudo chown -R -f john:www-data /var/www/html

Delete the default index page created with Nginx
::

  rm /var/www/html/index.nginx-debian.html

  sudo reboot

