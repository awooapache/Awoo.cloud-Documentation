# Changing Website Background

You can either just change the file in **/var/www/html/pomf/static/img/bg.png**

Or you can edit the CSS file in **/var/www/html/pomf/static/css/pomf.css**

In the css file just change  **&quot;background-image: url(&#39;grill.php&#39;), url(&#39;img/bg.png&#39;);&quot;**

to: **background-image: url(&#39;grill.php&#39;);**

Then edit the line above it &quot; **background-color: #F7F7F7;**&quot; to whatever you want.

After you&#39;re finished making changes you need to recompile by:

1.  **cd /var/www/html/pomf**

2.  **make**

3.  **make install DESTDIR=/var/www/html**

4.  **chown -R www-data:www-data /var/www/html**