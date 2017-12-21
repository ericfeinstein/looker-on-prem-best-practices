Since the Looker application relies heavily on the MySQL database for performance, it is important things are running smoothly on that as well. Below I have a few specific items to pay attention to. 

  


**MySQL Internal Database Maintenance:**

·**Rotate logs frequently**: if you are replicating MySQL, the logs for this task get significantly large and can tie up your hard disk space. In the case of a full hard disk, MySQL and/or Looker may not start.

·**Monitor slow queries**, you can setslow\_query\_log= 1 and this will log all slow queries, if many queries are flagging as slow, then consider adding more resources to your MySQL server. Review all of the options here:

o[http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)

·Monitor**packet size**between looker and the MySQL database. If needed, up themax\_allowed\_packetsize \(default is 1MB, this is too low\). This should also be updated on any slaves for replication.

·Set appropriate**caching**behavior:

oSetquery\_cache\_typeto 1

oSetquery\_cache\_sizeto something reasonable \(16 or more megabytes\)

o[http://dev.mysql.com/doc/refman/5.7/en/host-cache.html](http://dev.mysql.com/doc/refman/5.7/en/host-cache.html)

·Consider updating thebuffer\_pool\_size:

o[https://dev.mysql.com/doc/refman/5.7/en/innodb-buffer-pool-resize.html](https://dev.mysql.com/doc/refman/5.7/en/innodb-buffer-pool-resize.html)

·Turn off**DNS resolution**for faster response times withskip-name-resolve

o[https://developer.sugarcrm.com/2012/01/10/howto-turn-off-mysql-reverse-dns-lookup-to-speed-up-response-times/](https://developer.sugarcrm.com/2012/01/10/howto-turn-off-mysql-reverse-dns-lookup-to-speed-up-response-times/)

  


Also it is good to periodically monitor for MySQL related topics on[discourse](https://looker.com/search#stq=mysql)

  


