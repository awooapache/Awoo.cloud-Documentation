# Blocking Index Access to Users

You probably don&#39;t want your users to be able to access the entire list of files people have uploaded. By default they can very easily by going to yourdomain.com/files

To block this you need to edit your apache2.conf in **etc/apache2/apache2.conf**

You want to see if this code exists somewhere in the config, if it doesn&#39;t add it, if it&#39;s there and has &quot;index&quot; somewhere in it, delete it and replace it with this.

&lt;Directory /var/www/&gt;

 Options FollowSymLinks

 AllowOverride None

 Require all granted

&lt;/Directory&gt;