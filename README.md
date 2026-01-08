alter system set log_archive_dest_2=
'service=vprs.world LGWR ASYNC NOAFFIRM
 valid_for=(STANDBY_LOGFILE,STANDBY_ROLE)
 db_unique_name=<db_unique_name_of_0052>' scope=both;

alter system set log_archive_dest_state_2=enable scope=both;


select dest_id, status, error
from v$archive_dest_status
where destination is not null
order by dest_id;


select group#, thread#, sequence#, status
from v$standby_log
order by thread#, group#;


select * from v$archive_gap;
