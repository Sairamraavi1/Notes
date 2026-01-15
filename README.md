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


SELECT
  tablespace_name,
  ROUND(used_blocks * t.block_size / 1024 / 1024 / 1024, 2) AS used_gb,
  ROUND(free_blocks * t.block_size / 1024 / 1024 / 1024, 2) AS free_gb
FROM v$temp_space_header h
JOIN dba_tablespaces t
  ON h.tablespace_name = t.tablespace_name;

SELECT
  tablespace_name,
  ROUND(SUM(bytes)/1024/1024/1024,2) AS total_gb
FROM dba_temp_files
GROUP BY tablespace_name;


SELECT
  tablespace_name,
  ROUND(SUM(bytes_cached)/1024/1024/1024,2) AS used_gb
FROM v$temp_extent_pool
GROUP BY tablespace_name;



SELECT
  s.sid,
  s.serial#,
  s.username,
  u.tablespace,
  ROUND(u.blocks * t.block_size / 1024 / 1024 / 1024, 2) AS used_gb
FROM v$sort_usage u
JOIN v$session s ON u.session_addr = s.saddr
JOIN dba_tablespaces t ON u.tablespace = t.tablespace_name
ORDER BY used_gb DESC;



SELECT tablespace_name, file_name, bytes/1024/1024/1024 GB, autoextensible
FROM dba_temp_files;


