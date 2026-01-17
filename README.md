tail -200 impdp_set6_tables.log
grep -i "ORA-" impdp_set6_tables.log | tail -50
tail -200 nohup.out


select group#, thread#, bytes/1024/1024 as mb, status, archived, sequence#
from v$log
order by group#;

select group#, member from v$logfile order by group#, member;


select event, total_waits, time_waited/100 as seconds
from v$system_event
where event in ('log file switch (checkpoint incomplete)',
                'log file switch (archiving needed)',
                'log file sync',
                'log file parallel write')
order by seconds desc;


select * from v$recovery_file_dest;

select dest_id, status, error, destination
from v$archive_dest_status
where status <> 'VALID';


select directory_name, directory_path from dba_directories
where directory_name in ('DIR1','DIR2');


ls -lh <dir1_path>/expdp_165tables_*.dmp | head
ls -lh <dir2_path>/expdp_165tables_*.dmp | head
