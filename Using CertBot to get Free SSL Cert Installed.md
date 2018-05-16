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