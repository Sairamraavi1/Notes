select constraint_name, status
from dba_constraints
where owner='PSERP_R'
  and table_name='VBAK'
  and constraint_type='P';


select cc.constraint_name, cc.column_name, cc.position
from dba_cons_columns cc
where cc.owner='PSERP_R'
  and cc.table_name='VBAK'
  and cc.constraint_name = '<PK_NAME>'
order by cc.position;



where mandt = '100'
and vbeln in (...)


select *
from pserp_r.vbak
where mandt = '100'
and vbeln in ('0000123456','0000123457');
