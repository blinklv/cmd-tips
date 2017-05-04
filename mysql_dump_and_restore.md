# Mysql dump and restore

## Dump

```bash
mysqldump --single-transaction --host="127.0.0.1" --port="3306" --user="foo"  --password='bar'  database table > dump.sql
```

**NOTE**: Don't forget the *single-transaction* option, because most of time the storage engine of   
your table is **InnoDB**, this will ensure data consistency.

## Restore

```bash
mysql -h 127.0.0.1 -P 3306 -u "foo" --password="bar" -D database --default-character-set=utf8 < dump.sql
```

**NOTE**: Maybe you need change some contents of the *dump.sql* file, such as the first **DROP** statement.
