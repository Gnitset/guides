Install debian

Install dnsutils libcgi-fast-perl munin munin-node nagios-images nagios-plugins nagios3 php5-readline

Run
```
dpkg-statoverride --update --add nagios www-data 2710 /var/lib/nagios3/rw
dpkg-statoverride --update --add nagios nagios 751 /var/lib/nagios3
```

/etc/munin/apache24.conf:
```
--- apache24.conf.orig	2015-11-04 20:25:43.803547699 +0000
+++ apache24.conf	2015-11-04 20:34:12.985825392 +0000
@@ -1,12 +1,12 @@
 Alias /munin /var/cache/munin/www
 <Directory /var/cache/munin/www>
-        Require local
+        Require all granted
         Options None
 </Directory>
 
 ScriptAlias /munin-cgi/munin-cgi-graph /usr/lib/munin/cgi/munin-cgi-graph
 <Location /munin-cgi/munin-cgi-graph>
-        Require local
+        Require all granted
 	<IfModule mod_fcgid.c>
 	    SetHandler fcgid-script
 	</IfModule>
```

/etc/nagios3/apache2.conf
```
--- apache2.conf.orig	2015-11-04 18:52:24.184407760 +0000
+++ apache2.conf	2015-11-04 20:34:02.033769854 +0000
@@ -41,15 +41,15 @@
     </IfVersion>
 
     <IfVersion >= 2.3>
-        Require all denied
+        Require all granted
     </IfVersion>
 
-	AuthName "Nagios Access"
-	AuthType Basic
-	AuthUserFile /etc/nagios3/htpasswd.users
-	# nagios 1.x:
-	#AuthUserFile /etc/nagios/htpasswd.users
-	require valid-user
 </DirectoryMatch>
 
 <Directory /usr/share/nagios3/htdocs>
```

/etc/nagios3/cgi.conf
```
--- cgi.cfg.orig	2015-11-04 19:06:17.136167904 +0000
+++ cgi.cfg	2015-11-04 19:07:32.920517221 +0000
@@ -117,7 +117,7 @@
 # define this variable, anyone who has not authenticated to the web
 # server will inherit all rights you assign to this user!
  
-#default_user_name=guest
+default_user_name=guest
@@ -129,7 +129,7 @@
 # not use authorization.  You may use an asterisk (*) to
 # authorize any user who has authenticated to the web server.
 
-authorized_for_system_information=nagiosadmin
+authorized_for_system_information=*
@@ -141,7 +141,7 @@
 # an asterisk (*) to authorize any user who has authenticated
 # to the web server.
 
-authorized_for_configuration_information=nagiosadmin
+authorized_for_configuration_information=*
@@ -154,7 +154,7 @@
 # You may use an asterisk (*) to authorize any user who has
 # authenticated to the web server.
 
-authorized_for_system_commands=nagiosadmin
+authorized_for_system_commands=*
@@ -167,8 +167,8 @@
 # to authorize any user who has authenticated to the web server.
 
 
-authorized_for_all_services=nagiosadmin
-authorized_for_all_hosts=nagiosadmin
+authorized_for_all_services=*
+authorized_for_all_hosts=*
@@ -181,8 +181,8 @@
 # authorization).  You may use an asterisk (*) to authorize any
 # user who has authenticated to the web server.
 
-authorized_for_all_service_commands=nagiosadmin
-authorized_for_all_host_commands=nagiosadmin
+authorized_for_all_service_commands=*
+authorized_for_all_host_commands=*
``` 

/etc/nagios3/nagios.conf
```
--- nagios.cfg.orig	2015-11-04 19:08:22.044743123 +0000
+++ nagios.cfg	2015-11-04 19:10:56.393457349 +0000
@@ -23,7 +23,9 @@
 # Debian uses by default a configuration directory where nagios3-common,
 # other packages and the local admin can dump or link configuration
 # files into.
-cfg_dir=/etc/nagios3/conf.d
+cfg_dir=/etc/nagios3/needed-stuff
+cfg_dir=/etc/nagios3/own-setup
 
 # OBJECT CONFIGURATION FILE(S)
 # These are the object configuration files in which you define hosts,
@@ -142,7 +144,7 @@
 # you will have to enable this.
 # Values: 0 = disable commands, 1 = enable commands
 
-check_external_commands=0
+check_external_commands=1
```
