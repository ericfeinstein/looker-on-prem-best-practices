Since the Looker application relies heavily on the MySQL database for performance, it is important things are running smoothly on that as well. Below I have a few specific items to pay attention to. 



**MySQL Internal Database Maintenance:**

Rotate logs frequently: if you are replicating MySQL, the logs for this task get significantly large and can tie up your hard disk space. In the case of a full hard disk, MySQL and/or Looker may not start.

Monitor slow queries, you can set slow_query_log = 1 and this will log all slow queries, if many queries are flagging as slow, then consider adding more resources to your MySQL server. Review all of the options here:

- http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html

Monitor packet size between looker and the MySQL database. If needed, up the max_allowed_packet size (default is 1MB, this is too low). This should also be updated on any slaves for replication.

Set appropriate caching behavior:

- Set `query_cache_type` to 1

- Set `query_cache_size` to something reasonable (16 or more megabytes)

  - http://dev.mysql.com/doc/refman/5.7/en/host-cache.html

Consider updating the buffer_pool_size:

- https://dev.mysql.com/doc/refman/5.7/en/innodb-buffer-pool-resize.html

Turn off DNS resolution for faster response times with skip-name-resolve

- https://developer.sugarcrm.com/2012/01/10/howto-turn-off-mysql-reverse-dns-lookup-to-speed-up-response-times/



Also it is good to periodically monitor for MySQL related topics on discourse
