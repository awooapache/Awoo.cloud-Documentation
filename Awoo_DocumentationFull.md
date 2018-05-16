# Pomf (Awoo.cloud) Documentation

Documentation v1.0 Last updated 5/2/18

# **Getting a Domain Name and Creating DNS Records**

Get a name from [https://namecheap.com](https://namecheap.com) trust me, best prices and free WHOIS guard which you are going to need to ward off all the spam from the supposed spam resistant WHOIS registry.

- Make an account
- Buy a domain name
- Go to **Account-&gt;Dashboard**
- Click **Manage** on the right
- Click **Advanced DNS**
- Delete whatever records they have in there already
- Click **Add New Record**
- Make an A record with the host as &quot; **@**&quot; and the value is where you put the IP address of your server. Put TTL as automatic and save.
- Make another A record, under host type &quot; **www**&quot; and the value you put your server IP address again.
- Save and give it a few minutes and you are all set.

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

# Images in bottom right corner

Images for site located in **/var/www/html/img**

Images should be .png and 150x200-350 pixels

Code that deals with this is located in **/var/www/html/grill.php**

Just drop more images in the /img folder then make a new line with the name of the file in grill.php.

# Clearing Records in MariaDB/MySQL

Important: If you delete the files in the folder you specified your database records including hash values and link locations will remain. So if a file gets uploaded again it will error out if the record is not cleared from the database.

Starting MySQL/MariaDB:

**root@localhost: mariadb**

Selecting your database &quot;pomf&quot; if that&#39;s what you chose.

**MariaDB [(none]&gt; USE pomf;**

**MariaDB [pomf]&gt;  SHOW TABLES;**

Select whatever is in there, probably files.

**MariaDB [pomf]&gt;  TRUNCATE TABLE files;**

You can verify with:

**MariaDB [pomf]&gt;  SELECT \* FROM files;**

You can exit with CTRL+D

# Editing FAQ page

You can edit the FAQ page in **var/www/html/pomf/templates/faq/swig**

Anything changed in here gets changed when you recompile with make so it is better to modify this than it is to directly modify the HTML page. This file can be edited with any text editor.

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



# Using CertBot to get Free SSL Cert Installed

Having an SSL cert makes your site look more legit by having the green padlock on the top left of the search bar, but it also helps protect your users from having their data transmitted through insecure HTTP.

SSL certs used to be expensive to buy but now the EFF has a tool that lets you get them for free called CertBot.

If you are running the same system config as me (Apache and Debian 9) you can go here:

[https://certbot.eff.org/lets-encrypt/debianstretch-apache](https://certbot.eff.org/lets-encrypt/debianstretch-apache)

The instructions are well written there but here are the quick steps:

1. Go to your sources.list in **/etc/apt/sources.list** open it up in a text editor and add

**deb http://ftp.debian.org/debian stretch-backports main** then save it.

1. Run **apt-get update**
2. Run **sudo apt-get install python-certbot-apache -t stretch-backports**
3. Run **sudo certbot --authenticator webroot --installer apache** and run through the installer. It is very simple, only real choice you&#39;ll have to decide on is whether you want to force HTTPS or not. Enter 2 to enforce HTTPS would be my suggestion.
4. Run **service apache2 restart** and you should have a valid SSL cert installed for your site now.

# Blocking Index Access to Users

You probably don&#39;t want your users to be able to access the entire list of files people have uploaded. By default they can very easily by going to yourdomain.com/files

To block this you need to edit your apache2.conf in **etc/apache2/apache2.conf**

You want to see if this code exists somewhere in the config, if it doesn&#39;t add it, if it&#39;s there and has &quot;index&quot; somewhere in it, delete it and replace it with this.

&lt;Directory /var/www/&gt;

 Options FollowSymLinks

 AllowOverride None

 Require all granted

&lt;/Directory&gt;

# Changing the Navigation Menu Links

Edit nav.swig in **/var/www/html/pomf/templates/nav.swig**

You can just delete the whole line of the link you want to get rid of, or make a new link by just adding a line with: **&lt;li&gt;(a href=&quot;mylink.com&quot;&gt;My Link&lt;/a&gt;&lt;/li&gt;**

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

# Generating SSH Keys with PuTTYGen

Most worthwhile VPS providers will have authentication using SSH keys. You need to generate a private and public key set and give the public key to your provider. You can do this with PuTTY (which you will probably be using to connect anyways) [https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.70-installer.msi](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.70-installer.msi)

Once you have this installed this search for &quot;puttygen&quot; installed on your computer and open it.

Your provider might use different types of keys, but most likely it is RSA so you can keep it default.

- Click **Generate**
- Move your mouse around to generate the randomness in the key
- In the field **Key passphrase** choose a password. This will be used as kind of a second way to authenticate besides just having the key. Do not forget it.
- Once you do this press **Save public key** and put it somewhere on your PC. Then press **Save private key** and do the same.

Your provider is going to need your public key. **DO NOT GIVE ANYONE INCLUDING YOUR PROVIDER THE PRIVATE KEY.**

# Connecting to Server through PuTTY

If your VPS uses SSH authentication there are a few things you have to set to connect with Putty.

When you open PuTTY select **Session** and enter your server IP address under **Host Name** and set the **Port** to **22**. For connection type select **SSH**.

Then select under Category: **Connection** -&gt; **Data.** For Auto-Login username put what your VPS gives as the username (most likely &quot;root&quot;). This will just save some time logging in.

Also under Connection go to **SSH** -&gt; **Host Keys.** Click on the **Add Key** button and browse then select your private key you generated with PuTTYGen.

Go back to Session then press the **Save** button.

Launch the connection by double clicking your server IP address it saved.

# Preventing Banner Grabbing Attacks/Removing Server Info on Server Generated Pages

Apache by default has this dumb setting that puts the exact version number of your server on error pages, forbidden pages, etc. and also send this info to your website headers. To get rid of this you:

1. Navigate to **/etc/apache2/conf-enabled/security.conf**

2.  Search for **ServerSignature** and change the word from On to **Off**

3 **.  ** Search for **ServerTokens** and change the word from Full to **Prod**

4. Save then restart apache

5. Check your 404 pages by trying to go to a non-existent page, and then use [https://tools.geekflare.com/tools/http-header-test](https://tools.geekflare.com/tools/http-header-test) to make sure the exact version number is removed from your HTTP headers and instead just says Apache