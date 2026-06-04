We are testing DB Link validation between VPRS and PSERP.

Environment:

* VPRS and PSERP are Oracle 19.23.
* PSERP → VPRS DB Link works.
* VT3 → MT3 lower environment DB Link works.
* VPRS → PSERP has connectivity and authentication working, but query execution fails with:
  ORA-03113: end-of-file on communication channel.
* sqlnet.ora, tnsnames.ora, DB Link definition, Oracle version, and basic TNS connectivity were reviewed.
* Small/simple connectivity appears to work, but execution through the DB Link fails.
* VPRS has additional security layers such as CyberArk/Imperva.
* Newly created validation users were created in VPRS during read-write mode.
* Similar behavior was previously seen with newly created BODS users until security enablement/whitelisting was handled.

Current suspicion:
The issue may be due to one of these:

1. Newly created user not fully onboarded through CyberArk/Imperva.
2. Imperva/security layer terminating the VPRS → PSERP DB Link session.
3. SQL*Net timeout/session termination during query execution.
4. Distributed query execution plan issue over DB Link.
5. Remote PSERP server process crash or trace-level Oracle error.
6. User/profile/resource limit causing session termination.
7. Parallel query or large data movement across DB Link.

Please help identify what additional checks are needed to isolate the root cause and fix the issue.

Specific checks we need guidance on:

1. What should we check in PSERP alert log and trace files at the exact ORA-03113 timestamp?
2. What should we check in VPRS alert log and trace files?
3. How do we confirm whether Imperva/CyberArk blocked, reset, or terminated the session?
4. Should we test with the existing whitelisted SNF_ECC_BODS_USER to compare against the newly created validation user?
5. What SQLNET parameters should be aligned across VPRS and PSERP for Oracle 19.23?
6. Should we disable parallel query and retest?
7. How do we confirm whether the DB Link is failing only for large queries versus basic connectivity?
8. What database user/profile/resource limits should be checked?
9. What execution plan checks should be done for the failing query?
10. What is the safest workaround if DB Link remains unstable: local runtime schema in VPRS, filtered CTAS, export/import runtime schema into PSERP?

Please provide a step-by-step troubleshooting and fix plan.
