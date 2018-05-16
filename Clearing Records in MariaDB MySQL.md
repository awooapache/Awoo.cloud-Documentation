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