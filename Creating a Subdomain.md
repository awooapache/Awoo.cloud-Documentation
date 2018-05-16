# Creating a Subdomain

In namecheap go to Dashboard-&gt; Manage-&gt; Advanced DNS-&gt; Add new record

Create an A record and in the Host field put the name of what you want the subdomain to be. So if you want sub.mysite.com you would put sub in the host field.

Put your server IP in the value field then save.

Next you need to create an apache config file to let it know about this new sub domain.

Navigate to **etc/apache2/sites-available** to save time you can just make a copy of your pomf.conf in there and edit it.

Here is what it should be:

&lt;VirtualHost \*:80&gt;

ServerName sub.mysite.com

ServerAlias sub.mysite.com

ServerAdmin admin@mysite.com

#This is where you wanna keep the files of the subdomain

DocumentRoot /var/www/mysite/subdomain\_files

#REMOVE THESE TWO LINES IF YOU WANT NO LOGGING.

ErrorLog ${APACHE\_LOG\_DIR}/error.log

CustomLog ${APACHE\_LOG\_DIR}/access.log combined

#Do the following 3 lines if you want to force https, if not just delete them

RewriteEngine on

RewriteCond %{SERVER\_NAME} =sub.mysite.com

RewriteRule ^ https://%{SERVER\_NAME}%{REQUEST\_URI} [END,NE,R=permanent]

&lt;/VirtualHost&gt;

Save this file then run **a2ensite subdomain.conf**