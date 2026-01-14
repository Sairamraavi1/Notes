Description of change

Export 160 SAP ECC tables from Cascade staging source and import into PSERP Schema R (refresh transactional dataset) using Oracle Data Pump (expdp/impdp).

Business justification / Why

Required to refresh Schema R with the latest transactional data snapshot to support Elevate Mock 3 / downstream extraction and validation activities. Ensures Schema R contains current data needed by conversion and reporting processes.

Start Date & Time (PST)

[enter scheduled start] (ex: 1/16/26 9:00 PM)

End Date & Time (PST)

[enter scheduled end] (ex: 1/17/26 9:00 AM)
(Use your actual window. If unsure, put conservative.)

Rollback time

< 30 mins (or “< 1 hr”) depending on your plan

Duration of CR

Example: 12 hours (match your window)

Work Flow / Workstream

Choose the closest from the list (based on your sheet):

Data Loads / SAP / Oracle DBA / Elevate

Outage Required

Usually for Data Pump into schema:

Outage Required: No (if schema-level load only, no SAP app outage)

If they require read-only / restricted access to Schema R:

Outage Required: Partial / Schema R restricted

Impact

No customer-facing impact. Schema R will be under load; restrict non-essential queries/jobs during the import window.

Risk

Risk: Import duration may exceed window due to redo/log switch and storage throughput constraints. Mitigation: run during low activity window, controlled parallelism, monitor redo/archive space and I/O, and coordinate to avoid competing heavy jobs.

Implementation / High-level steps

Pre-checks

Confirm mount point free space for dump + logs

Confirm Schema R target tablespace free space

Confirm no conflicting jobs running

Export

Run expdp from Cascade staging for the 160 tables (consistent export as per plan)

Import

Truncate/drop target objects as per runbook

Run impdp into Schema R with agreed parallel and logging

Post

Validate object counts, row counts (sample validation), compile invalids

Confirm completion to stakeholders

Rollback / Backout plan

If import fails or data validation fails: stop Data Pump job, drop/revert newly imported objects in Schema R, and restore previous known-good Schema R baseline (either via prior export dump import or re-run last successful load).

Testing / Validation

Validate import completion, compare table counts/row counts for key tables, check invalid objects, confirm downstream extraction queries succeed.

Confluence / Runbook link

Put your runbook / diagram link if available (or “TBD”)
