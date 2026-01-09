SELECT username,
       userhost,
       terminal,
       returncode,
       timestamp
FROM   dba_audit_session
WHERE  returncode = 1017   -- ORA-01017: invalid username/password
ORDER  BY timestamp DESC;


SELECT dbusername,
       userhost,
       action_name,
       return_code,
       event_timestamp
FROM   unified_audit_trail
WHERE  return_code = 1017
ORDER  BY event_timestamp DESC;


cd /oracle/VE1/saptrace/audit

find . -name "*.aud" -newermt "2026-01-08 14:00" ! -newermt "2026-01-08 15:00" -print | xargs -r grep -i "ORA-1017"

