How I got rtGui working on Ubuntu 8.04 (Hardy Heron)

# Introduction #

This is a quick guide on how I got rtGui working on Ubuntu 8.04 (Hardy Heron)


# Details #

  * Download and install Ubuntu. http://www.ubuntu.com/GetUbuntu

  * Login and open up a new Terminal.
```
Applications -> Accessories -> Terminal
```

  * Install the necessary packages:
```
sudo apt-get install php5 php5-xmlrpc libapache2-mod-scgi rtorrent
```

  * Edit Apache default server config:
```
sudo gedit /etc/apache2/sites-enabled/000-default
```
  * Add the following lines, above the last "< /VirtualHost >" (or download example from http://rtgui.googlecode.com/files/000-default )
```
LoadModule scgi_module /usr/lib/apache2/modules/mod_scgi.so
SCGIMount /RPC2 127.0.0.1:5000
```
  * Restart Apache.  Check it starts OK - if not, check you've done the above steps OK.
```
sudo apache2ctl restart
```
  * Create .rtorrent.rc file - download example from http://rtgui.googlecode.com/files/.rtorrent.rc and save to your home directory - ~/.rtorrent.rc

  * Note: when you download this file, it might end up in 'dos' text format.  rtorrent won't like this, and complain with a message similar to "rtorrent: Error in option file: ~/.rtorrent.rc:16: Invalid start of name.".  To convert it to 'unix' format, first install the tools:
```
sudo apt-get install tofrodos
```
  * ..then convert it...
```
dos2unix .rtorrent.rc
```

  * Create directories for rtorrent to use, and change the ownership to the user that runs rtorrent:
```
sudo mkdir /Torrents /Torrents/Downloading /Torrents/Downloading/rtorrent.session /Torrents/Complete /Torrents/TorrentFiles /Torrents/TorrentFiles/Auto
sudo chown rtorrent /Torrents /Torrents/Downloading /Torrents/Downloading/rtorrent.session /Torrents/Complete /Torrents/TorrentFiles /Torrents/TorrentFiles/Auto
```
  * At this stage, check you can run rtorrent:
```
su - rtorrent  (optional - change to the user that runs rtorrent)
rtorrent
```
  * Leave rtorrent running, and open a new Tab.  (If at some point you want to close rtorrent, press CTRL-Q)
  * Download rtgui from http://code.google.com/p/rtgui/downloads/list
  * Extract files to webserver directory:
```
sudo tar xvzf ~/rtgui-x.x.x.tgz -C /var/www
```
  * If new install, use example config.php:
```
cd /var/www/rtgui/
sudo cp config.php.example config.php
```
  * Default settings in config should be OK, but just in case:
```
sudo gedit /var/www/rtgui/config.php
```
  * Now open your browser, and go to http://localhost/rtgui/
  * Enjoy!



# SUPPORT #
Please do not use these comments - use the Google Groups instead (link on project home page)