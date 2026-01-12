set lines 200 pages 200
col name format a90

SELECT file#,
       name,
       creation_time
FROM   v$datafile
WHERE  creation_time >= DATE '2026-01-10'
ORDER  BY creation_time;


select file#, name, creation_time
from v$datafile
where creation_time >= DATE '2026-01-10'
order by creation_time;
