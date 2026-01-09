SELECT username,
       userhost,
       terminal,
       returncode,
       timestamp
FROM   dba_audit_session
WHERE  returncode = 1017   -- ORA-01017: invalid username/password
ORDER  BY timestamp DESC;
