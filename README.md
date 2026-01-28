select sid, serial#, sql_id, event, wait_class
from v$session
where username = 'NODONOG1'
and status = 'ACTIVE';


select * 
from table(dbms_xplan.display_cursor('SQL_ID_HERE', null, 'ALLSTATS LAST'));


create index pserp_c.ix_lookup_lpad
on pserp_c.cnv_otcp_035_037_13_usgci_lookup ( lpad("KNA1-KUNNR",10,'0') );
