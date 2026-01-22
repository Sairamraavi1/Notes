SELECT to_char(s.logon_time,'YYYY-MM-DD HH24:MI:SS') logon_time,
       s.sid, s.serial#, s.username, s.machine, s.program, s.module,
       s.status, s.service_name
FROM   v$session s
WHERE  s.username = 'SVC_PRD_ALTERYX'
ORDER  BY s.logon_time DESC;


SELECT username, to_char(timestamp,'YYYY-MM-DD HH24:MI:SS') ts,
       returncode, os_username, userhost
FROM   dba_audit_session
WHERE  username='SVC_PRD_ALTERYX'
ORDER  BY timestamp DESC;

Check if Resource Manager is active:
SHOW PARAMETER resource_manager_plan;

If it’s not blank, get details:
SELECT * FROM dba_rsrc_plan_directives
WHERE plan = (SELECT value FROM v$parameter WHERE name='resource_manager_plan');

Also check if the user has a consumer group mapping:
SELECT * FROM dba_rsrc_consumer_group_privs
WHERE grantee='SVC_PRD_ALTERYX';

Profile limits that can disconnect quickly
Even if status is OPEN:
SELECT u.username, u.profile, p.resource_name, p.limit
FROM   dba_users u
JOIN   dba_profiles p ON p.profile=u.profile
WHERE  u.username='SVC_PRD_ALTERYX'
AND    p.resource_name IN ('SESSIONS_PER_USER','CONNECT_TIME','IDLE_TIME','FAILED_LOGIN_ATTEMPTS','PASSWORD_REUSE_TIME','PASSWORD_REUSE_MAX');

Logon triggers
SELECT owner, trigger_name, status
FROM   dba_triggers
WHERE  triggering_event LIKE '%LOGON%'
AND    status='ENABLED';

Profile limits that can disconnect quickly
lsnrctl status
lsnrctl services

Confirm service PSERP is registered and running
SELECT name, network_name, pdb, con_id
FROM   v$services
WHERE  name LIKE '%PSERP%';


SQL Developer-specific fix that usually resolves “reset” loops

Even when password is right, SQL Developer can “reset” due to SSL/JDBC settings.

Tell them to do one of these:

Option A: Use JDBC thin URL explicitly (bypasses some resolution issues)

In SQL Developer, Connection Type = Custom JDBC (if available), use:

jdbc:oracle:thin:@//pttnasvpr00052.unix.gsm1900.org:1527/PSERP
