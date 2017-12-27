**  
**Looker saves up to 90 days of history \(sometimes less\) for use by the application. It is truncated periodically to improve applicaiton performance. Here is how you can create a simple backup of that internal data.

_NOTE: This does not work with hypersql based setups._

### **Table setup**

**First, login to mysql and create a database for your history tables**

```
CREATE DATABASE looker_backup;
```

**Create a set of archive tables**

```sql
USE looker_backup
CREATE TABLE history_backup LIKE looker.history;
CREATE TABLE event_backup LIKE looker.event;
```

**The two most important are \`History\` and \`event\` other tables remove data such as scheduling and active queries. Likely they are not needed but you can repeat this process for them as well.**

**Looker should have created a primary key on the tables for you, you can see this with:**

```
SHOW INDEX FROM  history_backup
```

**If not, make sure to add a unique primary key \(we need this later\)**

```
ALTER TABLE history_backup ADD PRIMARY KEY(id);
```

### **User Setup**

**Create a user for the server to use to login \(I suggest different than your Looker credentials\)**

```
CREATE USER 'backup'@'localhost' IDENTIFIED BY 'backup_password';
GRANT SELECT ON looker.* TO 'backup'@'localhost';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP ON looker_backup.* TO 'backup'@'localhost';
```

Hint, we configure this user to login to mysql without specifying the password

  
back in your terminal, enter `vi ~/.my.cnf` and add these lines:

`[client]  
user=backup  
password=backup_password  
host=127.0.0.1`

Make sure to have the configuration file readable to you only.

**`chmod 0600 ~/.my.cnf`**

Once `~/.my.cnf` is created, simply typing mysql command will let you log in to this database as backup, and you no longer need to provide login password separately.

### Run the backup

For each table, configure a regular update.

```
REPLACE INTO looker_backup.history_backup SELECT * FROM looker.history;

```

Your result should look like this:

```
Query OK, 6 rows affected (0.00 sec)
Records: 6 Duplicates: 0 Warnings: 0
```

_Learn about REPLACE syntax: _[_https://dev.mysql.com/doc/refman/5.7/en/replace.html_](https://dev.mysql.com/doc/refman/5.7/en/replace.html)_. You can also add LOW\_PRIORITY flags to avoid locking, etc._  


You probably want to figure out your refresh interval and incorporate this into your backup script. This way you don’t backup the entire table. Here is how I would run a daily backup 

```
REPLACE INTO looker_backup.history_backup SELECT * FROM looker.history WHERE updated_at >= now() - INTERVAL 1 DAY;
```

###  **Automate**

To run this every day, add a cron entry like so \(for 12:01AM executions daily\):

open your crontab file \(terminal\): 

```
crontab -e
```

Then add the following two rows:

```
1 0 * * * mysql -h localhost -u backup -e "replace into looker_backup.history_backup select * from looker.historyWHEREupdated_at >= now() - INTERVAL1DAY"
```

```
1 0 * * * mysql -h localhost -u backup -e "replace into looker_backup.event_backup select * from looker.event WHERE updated_at >= now() - INTERVAL1DAY"
```

for help designing cron strings: https://crontab.guru/

Add more lines for additional tables or hit` cntl + x `to save

### Using this data.

If you haven’t already, its quite simple to create a connection to mysql via another \(read only\) user and begin modeling your internal Looker data! We highly recommend creating a read-replica for this purpose to avoid unnecessary load on the application database.

  


