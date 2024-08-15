###Userdir.conf
vim /etc/httpd/conf.d/userdir.conf
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    UserDir disabled
    UserDir enabled user1   #< UserDir disabled and then enabled with username only enables userdir for that user.

    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    UserDir /home/*    #< Location of user directories
</IfModule>
<Directory "/home/*">
    AllowOverride FileInfo AuthConfig Limit Indexes
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>
###

###SELINUX
setsebool -P httpd_enable_homedirs on 
restorecon -R /home 
###

###Make it accessible
chmod 711 /home/user1 #< Change user root dir permissions
chmod 755 /home/user1/* #< Change user directory content permissions so that it can be viewed
###

###Authentification
vim /etc/httpd/conf.d/auth_basic.conf
<Location ~ "/~.*>
    #SSLRequireSSL #< This disabled if not using SSL
    AuthType Basic
    AuthName "Basic Authentication"
    AuthUserFile /etc/httpd/conf/.htpasswd #< Location of passwords
    require valid-user
</Location>

htpasswd -c /etc/httpd/conf/.htpasswd user1  #< Set new password for user1
systemctl restart httpd 

