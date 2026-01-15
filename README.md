grep -n "Completed: alter database flashback on" alert_PSERP.log
grep -n "alter database flashback on" alert_PSERP.log


grep -n -B2 -A5 "alter database flashback on" alert_PSERP.log

TRANSFORM=DISABLE_ARCHIVE_LOGGING:Y


SELECT tablespace_name, file_id, file_name, status
FROM dba_temp_files
ORDER BY file_id;


ALTER TABLESPACE TEMP DROP TEMPFILE
'/oracle/VPR/sapdata1/temp_1/temp.data1';


ALTER TABLESPACE TEMP ADD TEMPFILE
'/oracle/VPR/sapdata1/temp_1/temp01.dbf'
SIZE 50G
AUTOEXTEND ON NEXT 1G MAXSIZE UNLIMITED;


SELECT tablespace_name, file_name, bytes/1024/1024/1024 GB, autoextensible
FROM dba_temp_files;


