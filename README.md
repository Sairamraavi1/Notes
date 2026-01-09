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
