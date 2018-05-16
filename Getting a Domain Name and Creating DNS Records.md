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