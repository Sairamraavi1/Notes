set lines 200 pages 200
col name format a80
select file#, name
from   v$datafile
where  name like '%UNNAMED%'
order  by file#;


set lines 200 pages 200
col file_name format a90

SELECT file_id,
       file_name,
       creation_time
FROM   dba_data_files
WHERE  creation_time >= DATE '2026-01-10'
ORDER  BY creation_time;

