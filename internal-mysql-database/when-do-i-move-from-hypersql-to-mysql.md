We recommend considering the move when your internal database exceeds 500mb, and allowing it to grow to no larger than 1GB.  This keeps the application performing well since HSQL runs in-memory and is not ideal for a large user base and you will run into issues if your servers run out of disk space.



To check the size, go to your looker installation folder and see how big your .db folder is. This folder is hidden for Looker's use so it won't show up in a standard ls command.

```
du -h .db
```

it will look something like this:

```
:~/looker$ du -h .db
4.0K	.db/looker.tmp
52M	.db
```

if the database exceeds 500M, consider moving to a MySQL backend

