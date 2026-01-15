grep -n "Completed: alter database flashback on" alert_PSERP.log
grep -n "alter database flashback on" alert_PSERP.log


grep -n -B2 -A5 "alter database flashback on" alert_PSERP.log

