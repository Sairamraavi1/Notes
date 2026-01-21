I see you’re using SQLPlus 11.2.0.4 against a 19c DB. It connects briefly, then drops with ORA-03114, and password change fails with ORA-01041 “hostdef extension doesn’t exist” — that points to an Oracle client/SQLPlus install mismatch on the workstation (PATH/ORACLE_HOME). Please use SQLcl or install Oracle 19c Instant Client (sqlplus) and retry with @//host:1527/PSERP. Once you’re on a 19c client, password change + login should work cleanly.


SELECT sid, serial#, username, machine, program, status, logon_time
FROM   v$session
WHERE  username = 'SVC_PRD_ALTERYX'
ORDER  BY logon_time DESC;


SELECT username, account_status, profile
FROM dba_users
WHERE username='SVC_PRD_ALTERYX';

SELECT owner, trigger_name, status
FROM dba_triggers
WHERE triggering_event LIKE '%LOGON%'
AND status='ENABLED';


Use correct EZCONNECT string
sqlplus user@"//pttnasvpr00052.unix.gsm1900.org:1527/PSERP"
