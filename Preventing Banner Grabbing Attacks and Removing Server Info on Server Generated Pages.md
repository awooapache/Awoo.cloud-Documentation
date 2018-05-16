# Preventing Banner Grabbing Attacks/Removing Server Info on Server Generated Pages

Apache by default has this dumb setting that puts the exact version number of your server on error pages, forbidden pages, etc. and also send this info to your website headers. To get rid of this you:

1. Navigate to **/etc/apache2/conf-enabled/security.conf**

2.  Search for **ServerSignature** and change the word from On to **Off**

3 **.  ** Search for **ServerTokens** and change the word from Full to **Prod**

4. Save then restart apache

5. Check your 404 pages by trying to go to a non-existent page, and then use [https://tools.geekflare.com/tools/http-header-test](https://tools.geekflare.com/tools/http-header-test) to make sure the exact version number is removed from your HTTP headers and instead just says Apache