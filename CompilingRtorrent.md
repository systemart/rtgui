# How I compiled rtorrent on Ubuntu 8.10

# Introduction #

This is how I got rtorrent compiled with XMLRPC-C on Ubuntu 8.10 (Intrepid), so that rtGui would display sizes of large files properly - without negative numbers.  The original problem is caused by the standard version of XMLRPC-C that is shipped with Ubuntu.

At the time of writing (Dec 2008), the current versions were rtorrent 0.8.2, libtorrent 0.12.2 and the SVN version of XMLRPC-C.

# Preparation #

Remove existing packages (if installed)...
```
~/$  sudo apt-get remove libxmlrpc-c3 rtorrent libtorrent11
```
Install required libraries...
```
~/$ sudo apt-get install libcurl3-dev libsigc++-2.0-0c2a libsigc++-2.0-dev 
```
Make a working directory and change into it...
```
~/$ mkdir build
~/build$ cd build
```


# XMLRPC-C #

Go to http://xmlrpc-c.svn.sourceforge.net/viewvc/xmlrpc-c/advanced/ and click "Download GNU tarball" at bottom of page, save the file advanced.tar.gz to your build/ directory.)
```
~/build$ tar xvzf xmlrpc-svn-advanced.tar.gz
~/build$ cd advanced
~/build$ autoconf
~/build$ ./configure --prefix=/usr
~/build$ make
~/build$ sudo make install
```
At this point I got an error - couldn't execute a file, so I changed the permissions on install-sh
```
~/build$ chmod a+x install-sh
~/build$ sudo make install
~/build$ cd ..
```


# libtorrent #
```
~/$ cd ~/build
~/build$ wget http://libtorrent.rakshasa.no/downloads/libtorrent-0.12.2.tar.gz
~/build$ tar xvzf libtorrent-0.12.2.tar.gz
~/build$ cd libtorrent-0.12.2
~/build/libtorrent-0.12.2$ ./configure --prefix=/usr
~/build/libtorrent-0.12.2$ make  ->!error!
```
At this point I got an error saying, amongst other things: "file\_list\_iterator.h:64: error: 'abs' is not a member of 'std'", so I downloaded the GCC4.3 patch from here from here: http://libtorrent.rakshasa.no/attachment/ticket/1266/libtorrent-gcc43-v2.patch, - select the 'Original Format' link at bottom of page and save it to the current directory.
```
~/build/libtorrent-0.12.2$ patch -p1 -i libtorrent-gcc43-v2.patch
~/build/libtorrent-0.12.2$ make
~/build/libtorrent-0.12.2$ sudo make install
~/build/libtorrent-0.12.2$ cd ..
~/build$ 
```


# rtorrent #
```
~/$ cd build/
~/build$ wget http://libtorrent.rakshasa.no/downloads/rtorrent-0.8.2.tar.gz
~/build$ tar xvzf rtorrent-0.8.2.tar.gz
~/build$ cd cd rtorrent-0.8.2/
~/build/cd rtorrent-0.8.2$ ./configure --prefix=/usr --with-xmlrpc-c 
~/build/cd rtorrent-0.8.2$ make 
```
Another error here: "text\_element\_value.cc:117: error: ‘gmtime’ is not a member of ‘std’".  Download patch from http://libtorrent.rakshasa.no/attachment/ticket/1267/rtorrent-gcc43.patch - select the 'Original Format' link at bottom of page, and save to current directory.
```
~/build/cd rtorrent-0.8.2$ patch -p1 -i rtorrent-gcc43.patch
~/build/cd rtorrent-0.8.2$ make
~/build/cd rtorrent-0.8.2$ sudo make install
```


# Finished #
Phew!
```
~/build$ cd ..
~/$ rtorrent 
```
Check version numbers at top of rtorrent display.