SHORT DESCRIPTION

Elevate: ADG Pause, Schema R Refresh, and DB Link Validation Testing in VPRS/PSERP Environment

DESCRIPTION

As part of Elevate activities, the team will perform a planned pause of Active Data Guard (ADG) replication in VPRS (Cascade DB) to refresh Schema R in PSERP using the latest SAPERP schema data from VPRS.

During the same controlled outage/testing window, the team will also conduct DB link validation and query optimization testing using the existing whitelisted service account to support automated validation activities for Sales Order and related validation objects.

Activities included:

* Pause ADG replication.
* Convert VPRS to snapshot standby mode.
* Export SAPERP schema data from VPRS.
* Refresh Schema R in PSERP.
* Resume ADG replication and validate synchronization.
* Execute DB link validation testing.
* Perform query optimization/tuning activities.
* Validate automated validation pipeline execution and working table generation.

JUSTIFICATION

This activity is required to refresh Schema R in PSERP with the latest source data from VPRS and to support ongoing Elevate validation and automation testing activities.

The refresh is necessary to:

* Ensure data consistency between VPRS and PSERP environments.
* Provide updated source data required for downstream validation and defect remediation testing.
* Support optimization and validation of automated DB link validation processes before production-aligned execution windows.
* Validate query execution behavior and working table generation in the actual VPRS/Cascade environment.
* Reduce execution and performance risks during upcoming cutover and validation activities.

Since VPRS operates in Active Data Guard (ADG) standby mode, a controlled pause of ADG replication is required to safely perform snapshot conversion and export activities.

This is a planned and controlled database activity executed within the Titan/VPRS staging environment with no direct production impact.

IMPLEMENTATION PLAN

Pre-Implementation Validation

1. Validate archive/apply synchronization between Primary, Titan, and VPRS.

2. Validate current database role/open mode:
   SELECT database_role, open_mode FROM v$database;

3. Validate managed standby/apply status:
   SELECT process, status FROM v$managed_standby;

4. Validate existing DB link connectivity using approved service account.

Cutover Activities

1. Pause managed standby recovery on VPRS.
2. Shutdown and mount database.
3. Convert VPRS to snapshot standby mode.
4. Open database in read/write mode.
5. Execute export of SAPERP schema objects required for Schema R refresh.
6. Refresh Schema R objects in PSERP environment.
7. Gather required statistics and validate object counts.
8. Execute DB link validation and query optimization testing using existing whitelisted service account.
9. Execute automated validation pipeline queries and validate working table generation.
10. Monitor TEMP, CPU, sessions, and query execution during testing.
11. Shutdown database and convert VPRS back to physical standby mode.
12. Resume managed standby recovery.
13. Validate ADG synchronization and recovery status.

ROLLBACK / BACKOUT PLAN

If any issue occurs during refresh or testing activities:

1. Stop active export/import and DB link testing sessions.

2. Drop/revert temporary working tables if required.

3. Shutdown VPRS instance.

4. Mount database.

5. Convert database back to physical standby mode.

6. Open database in READ ONLY WITH APPLY mode.

7. Resume managed standby recovery:
   ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

8. Validate:
   SELECT database_role, open_mode FROM v$database;

9. Validate standby apply health:
   SELECT process, status FROM v$managed_standby;

10. Confirm no significant lag or instability exists.

If Schema R refresh only partially completes, impacted objects can be reverted or refreshed again during next approved window.

RISK AND IMPACT ANALYSIS

Risk Level: Medium

Potential Risks:

* Temporary ADG replication pause during snapshot conversion.
* Increased CPU/TEMP utilization during export/import and query optimization activities.
* Long-running DB link validation queries may increase resource consumption.
* Extended outage window if refresh or testing exceeds estimated duration.

Mitigation:

* Activity limited to Titan/VPRS staging environment only.
* Continuous DBA monitoring throughout implementation.
* Existing validated service account will be used for DB link testing.
* Queries and sessions can be terminated immediately if abnormal utilization occurs.
* Full rollback procedure available to restore ADG standby configuration.

No direct production user impact is expected.

TEST PLAN

Objective:
Validate successful Schema R refresh, ADG recovery resumption, DB link functionality, and automated validation query execution.

Validation Steps:

1. Validate database role/open mode after recovery.
2. Validate Schema R object counts and sample data.
3. Validate invalid object count.
4. Validate ADG managed recovery status.
5. Validate DB link connectivity and query execution.
6. Validate working table generation through validation pipeline.
7. Validate query execution timings and stability.
8. Validate no ORA-03113 or DB link disconnect errors.
9. Validate standby synchronization health post activity.

Success Criteria:

* Schema R refresh completed successfully.
* ADG resumed successfully with healthy apply status.
* DB link queries completed successfully.
* Working tables generated successfully.
* No database instability observed.
* Validation queries completed within expected thresholds.

COMMUNICATION PLAN

* Provide implementation updates every 15–30 minutes during activity window.
* Notify stakeholders upon Schema R refresh completion.
* Notify stakeholders after DB link validation/testing completion.
* Escalate immediately if abnormal database or ADG behavior is identified.



ROLLBACK / BACKOUT PLAN

If any issue occurs during Schema R refresh, ADG recovery, or DB link testing activities, the following rollback steps will be performed immediately to restore the environment to stable standby mode.

1. Stop active export/import and DB link testing sessions.

2. Terminate long-running sessions if excessive resource utilization is observed.

3. Shutdown VPRS database:
   SHUTDOWN IMMEDIATE;

4. Mount database:
   STARTUP MOUNT;

5. Convert snapshot standby back to physical standby:
   ALTER DATABASE CONVERT TO PHYSICAL STANDBY;

6. Restart database and resume ADG apply:
   ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

7. Validate database status:
   SELECT database_role, open_mode FROM v$database;

Expected Result:

* DATABASE_ROLE = PHYSICAL STANDBY
* OPEN_MODE = READ ONLY WITH APPLY

8. Validate managed recovery/apply status:
   SELECT process, status FROM v$managed_standby;

9. Drop/revert incomplete temporary working tables if required.

10. Validate Schema R object availability and confirm database stability.

If Schema R refresh partially completes, impacted objects can be reverted or refreshed again during the next approved maintenance window.

Estimated rollback completion time: 30–45 minutes.

