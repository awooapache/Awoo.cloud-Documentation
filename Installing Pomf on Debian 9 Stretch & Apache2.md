# Installing Pomf on Debian 9 Stretch/ Apache2

After you get your Debian server installed setting up Apache is very easy.

Run: **sudo apt-get install apache2** Enter **Y** at the prompt.

Run: **ifconfig** record your IP address listed next to &quot;inet addr:&quot;

Open a web browser and enter that IP address into the search field. If you get a page that says &quot;Apache2 Debian Default Page&quot; you have successfully installed apache.

Next you want to move on to compiling the Pomf source code with Git. The dependencies can be annoying to install on Debian but here is what worked for me:

1. **sudo apt-get install curl python-software-properties**
2. **curl -sL https://deb.nodesource.com/setup\_8.x | sudo bash -**
3. **sudo apt-get install nodejs**
4. **sudo apt-get install mysql-server** (say N to first 2 installation questions, Y to the rest)
5. **sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql**
6. **service apache2 restart**
7. **sudo apt-get install git**
8. **cd /var/www/html**
9. **rm index.html**
10. **git clone https://github.com/pomf/pomf**
11. **rm -r expiry/ rm -r moe/**

You&#39;re going to have to edit a lot of files now so download [https://winscp.net/eng/download.php](https://winscp.net/eng/download.php) and login with your server details.

Took this straight from [https://catbox.moe/](https://catbox.moe/retard.html)  :

1. Open **dist.json** inside **/var/www/html/pomf** /.
2. Change **max\_upload\_size** to whatever you want it to be, with the file size in MB (that&#39;s BYTES, not bits.)
3. Change **Sitename** to whatever your pomf clone is called.
4. Change **siteUrl** to the FULL URL of your clone. i.e. https://catbox.moe/
5. Make abuseContact and infoContact the same email address that you use.

1. **mysql -p**. Log in with the password you made when you installed MySQL.
2. **CREATE DATABASE pomf;**
3. **quit**
4. **mysql -p pomf &lt; /var/www/html/pomf/mysql\_schema.sql**
5. Create a **new file** in **/etc/apache2/sites-available/** named pomf.conf
6. Put all of this inside it exactly. Replace [[]] with your info.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

&lt;VirtualHost \*:80&gt;

ServerName [[your domain]]

ServerAdmin [[your email]]

DocumentRoot /var/www/html

#REMOVE THESE TWO LINES IF YOU WANT NO LOGGING.

ErrorLog ${APACHE\_LOG\_DIR}/error.log

CustomLog ${APACHE\_LOG\_DIR}/access.log combined

&lt;Directory &quot;/var/www/html/files&quot;&gt;

php\_flag engine off

&lt;/Directory&gt;

&lt;/VirtualHost&gt;

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

Here is my config for reference:

&lt;VirtualHost \*:80&gt;

ServerName awoo.cloud

ServerAdmin awoocontact@protonmail.com

DocumentRoot /var/www/html/

#REMOVE THESE TWO LINES IF YOU WANT NO LOGGING.

ErrorLog ${APACHE\_LOG\_DIR}/error.log

CustomLog ${APACHE\_LOG\_DIR}/access.log combined

&lt;Directory &quot;/var/www/html/files&quot;&gt;

php\_flag engine off

&lt;/Directory&gt;

&lt;/VirtualHost&gt;

After you&#39;ve edited this you want to enable the config by **a2ensite pomf.conf**

Then run **service apache2 reload**

1. Open php.ini inside /etc/php/7.0/apache2/
2. Find and change upload\_max\_filesize and post\_max\_size to what you specified in dist.json. If you get confused, read the php docs.
3. Open settings.inc.php inside /var/www/html/pomf/static/php/includes/
4. Change the following to the correct details:

**define(&#39;POMF\_DB\_USER&#39;, &#39;root&#39;);**

**define(&#39;POMF\_DB\_PASS&#39;, &#39;&#39;);**

**define(&#39;POMF\_FILES\_ROOT&#39;, &#39;/var/www/html/files/&#39;);**

**define(&#39;POMF\_URL&#39;, http://yoursite.com /files/&#39;&#39;);**

1. **cd /var/www/html/pomf**
2. **make**
3. **make install DESTDIR=/var/www/html**
4. **mkdir /var/www/html/files**
5. **chown -R www-data:www-data /var/www/html**

If anything goes wrong you will get a log entry in: **/var/log/apache2/error.log** these are insanely helpful for figuring out what is wrong.

Most common issues I have had are:

- database username and  password in **settings.inc.php** were wrong
- forgot to save a config file and restart apache
- DocumentRoot location typed wrong
- Php.ini values incorrect