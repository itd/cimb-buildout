Install
================

FIRST: Set the Initial User !!!!
* Line 5 of buildout.cfg


Buildout
#------------
python bootstrap.py
./bin/buildout -c prod.cfg    # add -vvvvv if you want lots of verbiage

#Start-up Plone
#---------------
./bin/startcluster.sh

install site: cimb

Install XDV
install cimb.xdvtheme


Configure the XDV theme tool
=============================
Go to the control panel and change the paths to the XDV files.
All we should have to change is the prefix... anything before
``cimb.xdvtheme``



Fix the back-end display
==========================
The way this works, you'll go to the back-end Plone
to do edits.

In the ZMI, go to portal_css and check to see that
conditions on base.css, portlets.css, public.css
are set to:

    not:request/HTTP_X_XDV


Setup Apache
=======================

My Apache virtualhost looks like this:

<VirtualHost *:80>

    ServerName cimb.tool.net
    ServerAlias cimb.tool.org

	ServerAdmin webmaster@localhost
	DocumentRoot /web/cu/cimb/cimb-buildout/src/cimb.xdvtheme/cimb/xdvtheme/xdvparts
	<Directory /web/cu/cimb/cimb-buildout/src/cimb.xdvtheme/cimb/xdvtheme/xdvparts>
		Options Indexes FollowSymLinks MultiViews
		AllowOverride None
		Order allow,deny
		allow from all
	</Directory>

	ErrorLog /web/log/cimb.error.log
	# Possible values include: debug, info, notice, warn, error, crit,
	# alert, emerg.
	LogLevel warn
	CustomLog /web/log/cimb.access.log combined


  <Location />
  Order Allow,deny
  Allow from all
  </Location>

  #Header set X-UA-Compatible "IE=EmulateIE8"

  RewriteEngine On
  RewriteCond %{REQUEST_URI} !^/static/
  RewriteRule ^($|/.*) \
    http://127.0.0.1:20081/VirtualHostBase/http/%{SERVER_NAME}:80/cimb/VirtualHostRoot/$1 [P,L]
  #_vh_tool-zope
  #http://127.0.0.1:20071/VirtualHostBase/\
  #http://%{SERVER_NAME}:80/vid/VirtualHostRoot$1 [L,P]

</VirtualHost>



To Do
==========================
* Front Page
* The caching stuff
  * add squid or Varnish?
* The footer
