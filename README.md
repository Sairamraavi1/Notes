Data Migration Optimization – Simple Flow

Way 1 (Preferred): Staging by DBA → BODS → Schema R (No Export/Import)
Steps

Step 1
Pause ADG and convert the standby database to Snapshot Standby (READ WRITE mode).

Step 2
Based on BODS-provided queries, DBA creates staging tables (filtered data from multiple source tables).

Step 3
Validate staging tables

Row counts

Sample data

Basic sanity checks

Step 4
BODS team moves stagging tables data into Schema R (in a different database).

Step 5
Convert Snapshot Standby back to Physical Standby and re-initiate ADG, bringing the database back to READ ONLY mode.




Way 2 (Fallback): Staging by DBA → Export → Import → Schema R
Steps

Step 1
Pause ADG and convert the standby database to Snapshot Standby (READ WRITE mode).

Step 2
Based on BODS-provided queries, DBA creates staging tables.

Step 3
Validate staging tables

Row counts

Data sanity

Step 4
DBA exports staging tables from the Staging database.

Step 5
Convert Snapshot Standby back to Physical Standby and re-initiate ADG, bringing the database back to READ ONLY mode.

Step 6
DBA imports data into Schema R and creates indexes as required.


Main Prerequisites / Key Points
1) Query complexity drives time

Step 2 is the critical path

Execution time depends entirely on:

Number of tables involved

Join conditions

Filter logic

Data volume

Proactive indexing may not be feasible upfront

Performance will be tuned after observing query execution

2) Naming convention (must be defined before start)

Clear identification of staging tables

Example:

R<RunID>_<Object>_<SourceTable>

R20260218_SO_VBAK

3) Validation responsibility

DBA validates technical correctness

Business/BODS validates data correctness

4) Rollback (important clarity)

Snapshot Standby changes are automatically rolled back when converted back to Physical Standby


Previously, ADG was paused only for the export activity. In the new approach, ADG will be paused for a longer duration to accommodate staging and validation. The total pause window will increase and is directly dependent on the volume of data being staged.
