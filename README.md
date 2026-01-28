select sid, serial#, sql_id, event, wait_class
from v$session
where username = 'NODONOG1'
and status = 'ACTIVE';


select * 
from table(dbms_xplan.display_cursor('SQL_ID_HERE', null, 'ALLSTATS LAST'));


create index pserp_c.ix_lookup_lpad
on pserp_c.cnv_otcp_035_037_13_usgci_lookup ( lpad("KNA1-KUNNR",10,'0') );


select sql_id, child_number, executions, elapsed_time/1e6 elapsed_s
from v$sql
where sql_id = '06sn3t8w5j5rd';

select *
from table(dbms_xplan.display_cursor('06sn3t8w5j5rd', CHILD_NUMBER_HERE, 'ALLSTATS LAST'));


select *
from table(dbms_xplan.display_cursor('06sn3t8w5j5rd', 0, 'ALLSTATS LAST'));


-- confirm what SQL is running *right now* in that session
select s.sid, s.serial#, s.username, s.status,
       s.sql_id, s.sql_child_number,
       q.sql_text
from v$session s
join v$sqlarea q on q.sql_id = s.sql_id
where s.sid = 1039;


select piece, sql_text
from v$sqltext
where sql_id = (select sql_id from v$session where sid = 1039)
order by piece;
