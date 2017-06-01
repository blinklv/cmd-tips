# Get the sizes of the tables of a mysql

The following mysql statements are based on you have already login your Mysql.

## Get the size of the specific table

```sql
select
  table_name as 'Table',
  round(((data_length + index_length) / 1024 / 1024), 2) `Size in MB`
from
  information_schema.TABLES
where
  table_schema = "$DB_NAME" and 
  table_name = "$TABLE_NAME";
```

## List the size of every table in every database, largest first

```sql
select 
  table_schema as `Database`, 
  table_name as `Table`, 
  round(((data_length + index_length) / 1024 / 1024), 2) `Size in MB` 
from
  information_schema.TABLES 
order by 
  (data_length + index_length) desc;
```
