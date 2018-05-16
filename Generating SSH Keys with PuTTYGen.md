# Generating SSH Keys with PuTTYGen

Most worthwhile VPS providers will have authentication using SSH keys. You need to generate a private and public key set and give the public key to your provider. You can do this with PuTTY (which you will probably be using to connect anyways) [https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.70-installer.msi](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.70-installer.msi)

Once you have this installed this search for &quot;puttygen&quot; installed on your computer and open it.

Your provider might use different types of keys, but most likely it is RSA so you can keep it default.

- Click **Generate**
- Move your mouse around to generate the randomness in the key
- In the field **Key passphrase** choose a password. This will be used as kind of a second way to authenticate besides just having the key. Do not forget it.
- Once you do this press **Save public key** and put it somewhere on your PC. Then press **Save private key** and do the same.

Your provider is going to need your public key. **DO NOT GIVE ANYONE INCLUDING YOUR PROVIDER THE PRIVATE KEY.**