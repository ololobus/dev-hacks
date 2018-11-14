# PostgreSQL

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

#### Finding the size of your biggest relations 
```sql
SELECT nspname || '.' || relname AS "relation",
    pg_size_pretty(pg_relation_size(C.oid)) AS "size"
  FROM pg_class C
  LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace)
  WHERE nspname NOT IN ('pg_catalog', 'information_schema')
  ORDER BY pg_relation_size(C.oid) DESC
  LIMIT 10;
 ```
 More: https://wiki.postgresql.org/wiki/Disk_Usage

### Activity

```sql
SELECT * FROM pg_stat_activity;
```

### Investigate locks

```sql
-- View with readable locks info and filtered out locks on system tables

CREATE VIEW active_locks AS
SELECT clock_timestamp(), pg_class.relname, pg_locks.locktype, pg_locks.database,
       pg_locks.relation, pg_locks.page, pg_locks.tuple, pg_locks.virtualtransaction,
       pg_locks.pid, pg_locks.mode, pg_locks.granted
FROM pg_locks JOIN pg_class ON pg_locks.relation = pg_class.oid
WHERE relname !~ '^pg_' and relname <> 'active_locks';

-- Now when we want to see locks just type
SELECT * FROM active_locks;
```

More: http://blog.nordeus.com/dev-ops/postgresql-locking-revealed.htm


### VACUUM

Full vacuum
```sql
VACUUM (FULL, VERBOSE, ANALYZE);
```

### Backend pid
```sql
SELECT pg_backend_pid();
```

### Create large random dataset
Size approx. for `20000000`: `(8 + 8 + 8) * 20000000 bytes = 480 MB`.
Actualy on disk: `995 MB` (+ some space for row header).

```sql
CREATE TABLE large_test (num1 bigint, num2 double precision, num3 double precision);

INSERT INTO large_test (num1, num2, num3)
  SELECT round(random()*10), random(), random()*142
  FROM generate_series(1, 20000000) s(i);
```

Export if necessary
```sql
COPY large_test TO '/Users/username/Downloads/large_test.csv';
```
