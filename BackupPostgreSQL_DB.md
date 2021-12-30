1. Create a backup <br/>
```
pg_dump --file "C:\\Users\\Larry\\Documents\\PDP_0508_CAPTURE.backup" --host "172.18.0.21" --port "5432" --username "postgres" --verbose --role "postgres" --format=c --blobs --encoding "UTF8" --exclude-table-data=Attachment_hdr --exclude-table-data=Audit_det --exclude-table-data=Audit_hdr "PDP"
```


2. Restore <br/>
2.1 Force disconnect all connections
2.1.1 Connect to db server
```
psql -h 172.18.0.21 -p 5432 -U postgres
```
2.1.2 Run SQL
```
alter database "OBYMAIN" allow_connections=off;
SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity WHERE pg_stat_activity.datname = 'OBYMAIN' AND pid <> pg_backend_pid();
drop database "OBYMAIN";
create database "OBYMAIN";
```

2.2 Run Restore
```
pg_restore --host "172.18.0.21" --port "5432" --username "postgres" --role "postgres" --dbname "OBYMAIN" --verbose "C:\\Users\\Larry\\Documents\\OBYMAIN_11_30_2021.backup"
```

2.2.1 Or exclude specific tables
```
pg_restore -l "C:\\Users\\Larry\\Documents\\OBYMAIN_11_30_2021.backup" > restore.pgdump.list
```
Open generated file then add ; to the begin line of excluded tables.
Then run restore
```
pg_restore -L restore.pgdump.list --host "172.18.0.21" --port "5432" --username "postgres" --role "postgres" --dbname "OBYMAIN" --verbose "C:\\Users\\Larry\\Documents\\OBYMAIN_11_30_2021.backup"
```

3. Other (optional)
```
SELECT pg_size_pretty(pg_database_size('PL16831E'));
SELECT pg_size_pretty(pg_total_relation_size('audit_det'));
```
