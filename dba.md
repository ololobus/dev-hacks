# DBA

## PostgreSQL

### Count, preview and delete duplicates
```sql
SELECT count(id)
FROM (SELECT id, ROW_NUMBER() OVER (partition BY text1, text2 ORDER BY id) AS rnum FROM tus) t
WHERE t.rnum > 1;

SELECT id, text1
FROM (SELECT id, text1, ROW_NUMBER() OVER (partition BY text1, text2 ORDER BY id) AS rnum FROM tus) t
WHERE t.rnum > 1 limit 100;

DELETE FROM tus
WHERE id IN (
    SELECT id
    FROM (SELECT id, ROW_NUMBER() OVER (partition BY text1, text2 ORDER BY id) AS rnum FROM tus) t
    WHERE t.rnum > 1);
```

### Indexes and tables size

```sql
SELECT relname, relpages
FROM pg_class
ORDER BY relpages DESC;
```

```sql
SELECT relname, (relpages * 8) / 1024 AS size_mb
FROM pg_class ORDER BY relpages DESC LIMIT 10;
```

### Activity

```sql
select * from pg_stat_activity;
```
