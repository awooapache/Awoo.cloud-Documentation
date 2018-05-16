# Connecting to Server through PuTTY

If your VPS uses SSH authentication there are a few things you have to set to connect with Putty.

When you open PuTTY select **Session** and enter your server IP address under **Host Name** and set the **Port** to **22**. For connection type select **SSH**.

Then select under Category: **Connection** -&gt; **Data.** For Auto-Login username put what your VPS gives as the username (most likely &quot;root&quot;). This will just save some time logging in.

Also under Connection go to **SSH** -&gt; **Host Keys.** Click on the **Add Key** button and browse then select your private key you generated with PuTTYGen.

Go back to Session then press the **Save** button.

Launch the connection by double clicking your server IP address it saved.