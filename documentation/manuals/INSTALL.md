<div align="center" id="top">
	<a href="https://github.com/StateMapper/StateMapper#top" title="Go to the project's homepage"><img src="../../src/assets/images/logo/logo-black-big.png" /></a><br>
	<h3 align="center">INSTALLATION GUIDE</h3>
</div>

*[&larr; Project's homepage](https://github.com/StateMapper/StateMapper#top)*

-----


**Index:** [Requirements](#requirements) · [Installation](#installation) · [Daemon commands](#daemon-commands)


## Requirements:

StateMapper might work just fine on any [Debian-based](https://www.debian.org/derivatives/) Linux operating system, with the following software requirements:

* [PHP4+](http://php.net/) *(best PHP7+)*
* [MariaDB](https://mariadb.com/) with its [TokuDB plugin](https://mariadb.com/kb/en/library/tokudb/) *(though MySQL may be enough for local development)*
* [Apache](https://httpd.apache.org/) 2.2+ with [mod_rewrite](http://httpd.apache.org/docs/current/mod/mod_rewrite.html) enabled
* [cURL](http://php.net/manual/en/intro.curl.php)
* [pdftotext](https://poppler.freedesktop.org/) *(from Poppler)*
* [git](https://git-scm.com/docs/gittutorial) *(or simply download the files from this repository)*

* [IPFS](https://ipfs.io/ "InterPlanetary File System") *(optional)*
* [TOR](https://www.torproject.org/ "The TOR Project") *(optional)*


## Installation:

1. Install dependencies: *(if MySQL is installed, it will be replaced by MariaDB)*
   ```bash
   sudo apt-get install php7.0 apache2 libapache2-mod-php mariadb-plugin-tokudb php-mcrypt php-mysql curl poppler-utils git
   ```
2. Install TokuDB following [these instructions](https://mariadb.com/kb/en/library/enabling-tokudb/). 

3. Secure your database entering ```mysql_secure_installation```.

4. Enable Apache's mod_rewrite: *(replace ```www-data``` and ```/var/www/html``` if convenient)*
   ```bash
   sudo a2enmod rewrite		# enable mod_rewrite
   sudo service apache2 restart 	# restart Apache
   ```
   
   If mod_rewrite is still not working, try editing ```/etc/apache2/apache2.conf``` to set ```AllowOverride``` to ```All``` inside of ```<Directory /var/www/>```. Then restart Apache again.

5. OPTIONAL: Install IPFS following [these instructions](https://ipfs.io/docs/install/). Then enter:

   ```bash
   ipfs init
   ipfs daemon& 			# wait 3 seconds and press Ctrl+L to clear the screen
   ipfs cat /ipns/...... 		# shoud print something if IPFS is well configured
   ```

6. OPTIONAL: Install TOR following [these instructions](https://www.torproject.org/docs/debian.html.en).  
   
   Then edit ```/etc/tor/torrc```. Uncomment ```ControlPort 9051```, uncomment ```CookieAuthentication 0``` and set it to 1 (```CookieAuthentication 1```). Save and close. Then enter:  

   ```bash
   sudo service tor restart	 		# make sure TOR is running
   curl ifconfig.me/ip				# should print your real IP
   torify curl ifconfig.me/ip			# should print another IP
   print 'AUTHENTICATE ""\r\nsignal NEWNYM\r\nQUIT' | nc 127.0.0.1 9051
   torify curl ifconfig.me/ip 			# should print yet another IP
   ```

7. Clone this repository to a dedicated folder in your Apache's root folder: *(most probably ```/var/www/html```)*

   ```bash
   git clone https://github.com/StateMapper/StateMapper /var/www/html/statemapper
   ```
   *Alternatively, if you already have the files, you can simply extract them to ```/var/www/html/statemapper```.*


8. Fix Apache permissions: *(replace ```www-data``` and ```/var/www/html``` if convenient)*
   ```bash
   sudo chgrp -R www-data /var/www/html
   sudo find /var/www/html/statemapper -type d -exec chmod g+rx {} +
   sudo find /var/www/html/statemapper -type f -exec chmod g+r {} +
   ```
   
9. RECOMMENDED: Create an ```smap``` alias to access the CLI API easily from anywhere by entering:

   ```bash 
   echo 'alias smap="/var/www/html/statemapper/scripts/statemapper "' >> ~/.bashrc
   source ~/.bashrc		# read ~/.bashrc again
   smap				# should print the CLI help
   ```
   
   *Disclaimer: all ```smap``` calls require root login because PHP requires to be executed with the same user as the Apache server to be able to read-write files correctly.*

10. Open a browser, navigate to http://localhost/statemapper/ and follow the instructions.

11. RECOMMENDED: Edit ```config.php``` and change the constants according to your needs (follow the instructions in comments).


## Daemon commands:

If you want the spiders to be able to start, it is required to start the daemon with ```smap daemon start```. Here are the available daemon commands:

```bash
smap daemon [start] 		# start the daemon in the background
smap daemon restart 		# restart the daemon
smap daemon status 		# print the daemon's status
smap daemon -d 			# start it in debug mode (do not daemonize, print information)
smap daemon -dd 		# start it in advanced debug mode (same as -d but printing SQL queries too)
smap daemon stop 		# stop it smoothly (wait for the workers)
smap daemon kill 		# kill it (for emergencies only)
```

If you don't see the spiders' start/stop buttons on the rewind pages, please make sure you are logged in (from the copyright's menu).

To automatically start the daemon at every server startup, add ```smap daemon start``` at the beginning of your ```/etc/rc.local``` file (only recommended for production).



-----

*[&larr; Project's homepage](https://github.com/StateMapper/StateMapper#top) · Copyright &copy; 2017 [StateMapper.net](https://statemapper.net) · Licensed under [GNU GPLv3](../../COPYING) · [&uarr; top](#top)* <img src="[![Bitbucket issues](https://img.shields.io/bitbucket/issues/atlassian/python-bitbucket.svg?style=social" align="right" /> <img src="http://hits.dwyl.com/StateMapper/StateMapper.svg?style=flat-square" align="right" />

