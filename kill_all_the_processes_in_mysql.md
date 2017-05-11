# Kill all the processes in mysql

```bash
$ processes=$(mysql -h host -P port -u user --password=password -D database --default-character-set=utf8 -e "select concat('KILL ',id,';') from information_schema.processlist;")

$ mysql -h host -P port -u user --password=password -D database --default-character-set=utf8 -e "$processes"
```

If you only want to kill the processes wasting time, you can change the first command as following.

```bash
$ processes=$(mysql -h host -P port -u user --password=password -D database --default-character-set=utf8 -e "select concat('KILL ',id,';') from information_schema.processlist where time > 180;")
```

It will kill the processes running time more than 3 minutes.
