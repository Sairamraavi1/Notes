SHORT DESCRIPTION

Elevate: VPRS DB Link Validation and Query Optimization Testing using Service Account in Titan Environment

DESCRIPTION

As part of Elevate validation activities, the team will conduct controlled DB link validation and query optimization testing in the Titan/VPRS staging environment using the existing whitelisted service account.

The activity includes:

* Validation of DB link connectivity between VPRS (Cascade DB) and PSERP.
* Query optimization and tuning for automated validation pipelines.
* Validation of lookup table access from PSERP through DB link.
* Generation and validation of working tables within VPRS for downstream automated functional validation testing.

No production data modifications will occur. Testing will be performed in Titan/VPRS staging environment only.

JUSTIFICATION

This activity is required to validate and optimize the automated functional validation process planned for upcoming Elevate testing and cutover activities.

The testing is necessary to:

* Validate DB link connectivity and query execution behavior in actual VPRS/Cascade environment.
* Fine-tune long-running validation queries prior to production-aligned execution windows.
* Validate automated validation pipeline functionality for Sales Order and related validation objects.
* Reduce execution risks during planned cutover validation activities.
* Confirm that the existing whitelisted service account can support optimization/testing activities prior to final onboarding of the newly provisioned service account.

This is a planned and controlled testing activity executed within the Titan staging environment with no direct impact to production users or live production processing.

IMPLEMENTATION PLAN

Pre-Implementation Validation

1. Confirm VPRS database status:
   SELECT database_role, open_mode FROM v$database;

2. Validate ADG status and lag:
   SELECT process, status FROM v$managed_standby;

3. Validate DB link connectivity:
   SELECT * FROM dual@<DBLINK_NAME>;

4. Confirm existing whitelisted service account connectivity.

Implementation Activities

1. Team will connect to VPRS/Cascade environment using approved accounts.
2. Execute DB link validation queries between VPRS and PSERP.
3. Pull required lookup/configuration tables through DB link.
4. Execute query optimization and tuning activities for automated validation queries.
5. Create and validate temporary/working tables required for validation pipeline processing.
6. Monitor database resource utilization during testing activity.
7. Capture execution timings and validate query performance improvements.
8. Coordinate with BODS and validation teams during testing window.

Monitoring Activities

* Monitor database sessions.
* Monitor TEMP usage.
* Monitor CPU and I/O utilization.
* Monitor long running SQL execution.
* Validate no impact to existing export/import activities.

ROLLBACK / BACKOUT PLAN

If any instability, performance issue, or unexpected behavior is identified:

1. Stop active validation/query testing sessions.

2. Drop temporary working tables created during testing if required.

3. Disable or disconnect active DB link sessions if needed.

4. Validate database stability:
   SELECT database_role, open_mode FROM v$database;

5. Validate ADG status remains healthy:
   SELECT process, status FROM v$managed_standby;

6. Confirm no blocking or long-running residual sessions remain active.

7. Return environment to standard operational state.

No permanent application or production data changes are involved in this activity.

RISK AND IMPACT ANALYSIS

Risk Level: Low to Medium

Potential Risks:

* Increased database resource utilization during query optimization testing.
* Temporary performance impact within Titan/VPRS staging environment.
* Long-running queries may consume TEMP or CPU resources.

Mitigation:

* Activity will be monitored throughout execution.
* Testing limited to staging/Titan environment only.
* Existing whitelisted service account already previously validated.
* DBA team will monitor resource utilization continuously.
* Queries can be terminated immediately if abnormal resource consumption is observed.

No direct production impact is expected.

TEST PLAN

Objective:
Validate DB link functionality, query execution stability, and automated validation pipeline optimization in VPRS/Titan environment.

Validation Steps:

1. Validate successful DB link connectivity.
2. Validate lookup table access through DB link.
3. Validate query execution completion without ORA-03113 or connectivity failures.
4. Validate working table creation and downstream query execution.
5. Compare query execution timings before and after optimization.
6. Validate no abnormal TEMP, CPU, or session growth.
7. Validate ADG/database status remains healthy throughout testing.

Success Criteria:

* DB link queries complete successfully.
* No ORA-03113 or DB link disconnect errors observed.
* Working tables created successfully.
* Query execution timings improved or validated.
* No database instability observed.
* ADG/database status remains healthy post testing.

COMMUNICATION PLAN

* Provide updates every 30 minutes during testing window.
* Notify stakeholders upon activity completion.
* Escalate immediately if abnormal database behavior is identified.
