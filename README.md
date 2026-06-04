We are troubleshooting an Oracle DB Link issue between VPRS and PSERP databases and need a deep technical investigation from a pure Oracle/DB Link perspective.

Environment Details:

* Oracle Database Version:

  * VPRS: 19.23.0.0.0
  * PSERP: 19.23.0.0.0
  * VT3: 19.23.0.0.0
  * MT3: 19.23.0.0.0

* VPRS and PSERP are on the same physical host.

* VT3 to MT3 DB Link testing works successfully.

* PSERP to VPRS DB Link testing works successfully.

* Only VPRS to PSERP DB Link execution is failing.

Observed Error:

* ORA-03113: end-of-file on communication channel

Observed Behavior:

* DB Link connectivity works.
* TNS resolution works.
* Authentication/login succeeds.
* Query starts execution.
* During execution, session disconnects with:
  ORA-03113: end-of-file on communication channel

What Has Already Been Checked:

1. DB Link creation syntax
2. tnsnames.ora configuration
3. sqlnet.ora configuration
4. Oracle version compatibility
5. Listener connectivity
6. Basic DB Link connectivity
7. Reverse DB Link direction
8. Lower environment DB Link testing
9. SQLNET.ALLOWED_LOGON_VERSION values
10. DEFAULT_SDU_SIZE settings
11. SQLNET.EXPIRE_TIME settings

Current sqlnet.ora settings include:

* SQLNET.EXPIRE_TIME=10
* DEFAULT_SDU_SIZE=32768
* SQLNET.ALLOWED_LOGON_VERSION_SERVER=10/12
* SQLNET.ALLOWED_LOGON_VERSION_CLIENT=10/12

Issue Specifics:

* PSERP → VPRS works
* VT3 → MT3 works
* VPRS → PSERP fails
* Connectivity exists but execution disconnects
* Failure happens during query execution, not during login
* Failure occurs through DB Link queries
* Queries involve large SAP ECC source tables and filtered subset validation queries
* Some queries execute for some time before failing
* Query performance through DB Link is also significantly slower than expected

We need a technical investigation strictly from an Oracle DB Link / SQLNET / distributed query execution perspective.

Please help analyze:

1. What are the most likely Oracle-side causes of:
   ORA-03113: end-of-file on communication channel
   specifically during distributed DB Link query execution?

2. Since connectivity and authentication succeed, what components should be investigated next?

   * SQLNET session handling?
   * Distributed transaction layer?
   * Parallel query over DB Link?
   * Remote session termination?
   * Oracle bug?
   * SQL execution plan?
   * Distributed optimizer behavior?

3. What exact checks should be performed on:

   * VPRS alert logs
   * PSERP alert logs
   * Listener logs
   * Trace files
   * ADRCI diagnostics

4. What queries should be executed to investigate:

   * Distributed query execution
   * DB Link session behavior
   * Remote session failures
   * Wait events
   * Parallel slaves
   * SQL execution plan issues
   * Resource limits
   * Session kills
   * Network/session timeouts

5. What Oracle parameters should be verified/aligned between VPRS and PSERP?
   Especially related to:

   * SQLNET
   * Distributed transactions
   * Parallel execution
   * Open links
   * Session/process limits
   * SDU/TDU
   * Dead connection detection

6. Could large distributed joins or CTAS over DB Link trigger ORA-03113 even when the DB Link itself is healthy?

7. What is the best way to isolate whether:

   * the issue is query-specific,
   * DB Link execution-specific,
   * SQLNET/session-specific,
   * optimizer-related,
   * or Oracle process crash-related?

8. Should we test with:

   * parallel disabled,
   * small table queries,
   * CTAS locally,
   * DRIVING_SITE hints,
   * remote filtering,
   * materialized subset tables,
   * or no distributed joins?

9. What Oracle bugs or known 19c distributed query issues should be checked for ORA-03113 during DB Link execution?

10. What would be the recommended step-by-step troubleshooting sequence to isolate and fix this issue?

Please provide:

* detailed root-cause possibilities,
* exact investigation commands,
* Oracle views to check,
* trace locations,
* recommended SQLNET settings,
* optimizer/distributed query checks,
* and practical validation tests.
