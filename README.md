SELECT s1.username || '@' || s1.machine || ' ( SID=' || s1.sid || ' ) is blocking ' || s2.username || '@' || s2.machine || ' ( SID=' || s2.sid || ' ) ' AS blocking_status
FROM gv$lock l1, gv$session s1, gv$lock l2, gv$session s2
WHERE s1.sid = l1.sid AND s2.sid = l2.sid AND l1.BLOCK = 1 AND l2.request > 0 AND l1.id1 = l2.id1 AND l2.id2 = l2.id2;
