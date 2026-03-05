SELECT originating_timestamp, message_text
FROM v$diag_alert_ext
WHERE message_text LIKE '%Managed Standby Recovery%'
ORDER BY originating_timestamp DESC;


SELECT originating_timestamp, message_text
FROM v$diag_alert_ext
WHERE message_text LIKE '%snapshot standby%'
ORDER BY originating_timestamp DESC;
