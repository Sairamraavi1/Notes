set lines 200 pages 200
col name format a80
select file#, name
from   v$datafile
where  name like '%UNNAMED%'
order  by file#;
