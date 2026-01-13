select event, count(*)
from v$session
where program like 'DW%'
   or program like '%impdp%'
group by event
order by 2 desc;
