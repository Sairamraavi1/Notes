Object and Row Count Verification

Validate that all 160 SAP tables specified in the export list are included.

Perform spot-check row counts for a representative subset of tables.

Confirm exported object count matches the defined table list.

Pass Criteria:

All expected tables are present in the export.

Row counts are within expected variance for sampled tables.

3. Import Readiness Validation (Cascade)

Validate dump files are accessible from Cascade.

Ensure sufficient disk space and permissions for import.

Confirm no corruption or access issues with dump files.

Pass Criteria:

Dump files are readable and accessible.

No file system or permission errors.

4. ADG Replication Validation

Confirm ADG replication is re-enabled post-export.

Validate redo transport and apply are functioning normally.

Verify no redo apply gap exists between primary and standby.

Pass Criteria:

Managed Recovery Process (MRP) is running.

Redo apply gap = 0.

No replication-related errors in alert logs.

Test Completion Criteria

Testing is considered complete when:

Export is successful and validated.

Dump files are verified and ready for import.

ADG replication is fully restored and synchronized.

No database or replication errors remain.
