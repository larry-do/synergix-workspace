alter database "OBA_MAIN" allow_connections=off;
SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity WHERE pg_stat_activity.datname = 'OBA_MAIN' AND pid <> pg_backend_pid();
drop database "OBA_MAIN";
create database "OBA_MAIN";
;

pg_dump --file "D:\\OBA_MAIN_0508_CAPTURE.backup" --host "localhost" --port "5432" --username "postgres" --verbose --role "postgres" --format=c --blobs --encoding "UTF8" "OBA_MAIN"

pg_restore --host "localhost" --port "5432" --username "postgres" --role "postgres" --dbname "OBA_MAIN" --verbose "D:\\OBA_MAIN_0508_CAPTURE.backup"

alter database "OBA_CTRL" allow_connections=off;
SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity WHERE pg_stat_activity.datname = 'OBA_CTRL' AND pid <> pg_backend_pid();
drop database "OBA_CTRL";
create database "OBA_CTRL";

pg_restore --host "localhost" --port "5432" --username "postgres" --role "postgres" --dbname "OBA_CTRL" --verbose "D:\\OBA_CTRL_0508_CAPTURE.backup"

pg_dump --file "D:\\OBA_CTRL_0508_CAPTURE.backup" --host "localhost" --port "5432" --username "postgres" --verbose --role "postgres" --format=c --blobs --encoding "UTF8" "OBA_CTRL"