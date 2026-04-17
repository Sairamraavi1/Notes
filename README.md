Short Description (same field as screenshot)

👉 Paste this:

Increase memory on host 0052 to support PSERP/VPRS export, import, indexing, and cutover testing activities
🔹 Step 4: Description field

👉 Paste this:

Request to increase memory on host 0052 to improve performance for project-related database activities on PSERP and VPRS.

Host 0052 is currently used for export/import, schema refresh, indexing, and validation activities. These operations are memory intensive and currently facing resource pressure.

The requested change is to increase memory on 0052 to support ongoing dry runs, performance testing, and cutover preparation.

This is an infrastructure-level change only. No application changes involved.

If required, server restart will be performed during approved change window.
✅ Now move to Planning Tab (VERY IMPORTANT)
🔹 Step 5: Justification (as per your screenshot)

👉 Paste this:

Host 0052 is supporting critical database activities for PSERP and VPRS including export/import, indexing, schema refresh, and cutover rehearsal.

Current memory is insufficient for high-volume operations, causing performance bottlenecks and longer execution times.

Increasing memory will improve performance, reduce resource contention, and support project timelines for testing and cutover.

This change is required to ensure stable and efficient execution of project activities.
🔹 Step 6: Implementation Plan

👉 Paste this (structured like your screenshot):

1) Validate current memory allocation on host 0052
2) Confirm target memory size with infrastructure team
3) Notify stakeholders about change window
4) Ensure no active DB activities running on host
5) Login to server management console (HMC/Infra tool)
6) Increase memory allocation for host 0052
7) Restart server if required
8) Bring server back online
9) Verify OS-level memory update
10) Validate PSERP and VPRS connectivity
11) Confirm database availability
12) Capture evidence of memory increase
🔹 Step 7: Risk and Impact Analysis

👉 Paste this:

Risk: Medium

This activity may require server restart, leading to temporary unavailability of services on host 0052.

Impact if not implemented:
Database operations such as export/import and indexing will continue to face performance issues, increasing risk to project timelines and cutover readiness.

Risk is mitigated by performing activity in planned window and validating services post change.
🔹 Step 8: Backout Plan

👉 Paste this:

1) If any issue occurs, inform infrastructure team immediately
2) Revert memory configuration to previous state
3) Restart server if required
4) Verify system stability
5) Validate PSERP and VPRS connectivity
6) Confirm database availability
7) Notify stakeholders of rollback

Estimated rollback time: 30-60 minutes
🔹 Step 9: Test Plan

👉 Paste this:

1) Verify host 0052 is accessible
2) Check updated memory at OS level
3) Validate all services are running
4) Confirm PSERP and VPRS database connectivity
5) Perform basic DB validation
6) Capture evidence of successful change
