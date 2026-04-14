suggestions are valid as test options, but they are not guaranteed to give a drastic reduction in our case. Splitting the export/import may help somewhat if we are currently underutilizing resources, but if the real bottleneck is target-side import IO, tablespace growth, and index creation, then splitting alone will not magically solve the problem. ACCESS_METHOD=DIRECT_PATH is also worth testing, but Oracle recommends AUTOMATIC because Data Pump already chooses the most efficient unload method when possible, so forcing DIRECT_PATH does not guarantee improvement. Transportable can be much faster only when the object placement and tablespace design support it; otherwise it is not a simple drop-in replacement.”

Does splitting export into 2 and then importing help?

Answer: maybe a little, not necessarily a lot.

Why:

Splitting can help when a single job is not fully using CPU, worker processes, or available channels.
But if your biggest bottleneck is already import-side write IO, tablespace extension waits, and index build time, then 2 import jobs can just create more contention on the target.
So this is a testable optimization, not a guaranteed solution.
Practical expectation
Best case: some improvement from better concurrency and better separation of big/small tables.
Realistic case in your situation: small to moderate gain, not a dramatic cut.
If import is already saturated on IO, you may see little benefit or even no benefit.

So the management-safe answer is:

“Splitting into 2 jobs can help only if we are currently not fully utilizing resources. But if the target import side is already IO-bound, then the gain will be limited.”

Does ACCESS_METHOD=DIRECT_PATH really help?

Answer: it can help in some cases, but not always.

Oracle says ACCESS_METHOD is provided so you can try an alternative method if the default does not work as desired, and Oracle explicitly recommends AUTOMATIC whenever possible because Data Pump can automatically choose the most efficient method for each table. Also, ACCESS_METHOD is not valid for transportable tablespace jobs.

What that means for you
If you are already using normal Data Pump export, Oracle may already be choosing direct path where possible.
Forcing ACCESS_METHOD=DIRECT_PATH may help for some tables, but it is not a guaranteed global speed-up.
It usually affects the export/unload side, not the full end-to-end import/index problem.
How much improvement?

There is no official fixed number from Oracle like 20% or 50%.
A safe answer is:

“It may improve export performance for eligible tables, but we should treat it as a test parameter, not as a guaranteed major improvement. Any gain is workload-dependent.”

So do not promise management that this alone will cut many hours.

What else did he say about transportable tablespaces / schema-level transport?

He is basically describing a transportable-style movement, where instead of doing full unload/reload of table data, you move metadata plus copy the underlying datafiles. That is why he is saying it could be much faster. Oracle documents exactly that for transportable tables and tablespaces: metadata is moved by Data Pump, and the actual data is moved by copying datafiles.

But there are important limits:

The involved tablespaces must be read-only.
Transportable only works cleanly when the storage layout supports it.
If the required 160 tables are mixed with thousands of other tables in the same tablespace, then transporting that tablespace would bring much more than just your required objects, which is exactly the concern raised in your call.
Oracle also notes that all storage for selected objects must fit transportable rules; objects can’t straddle transportable and non-transportable placement arbitrarily in a network import.
Will transportable finish in 1–2 hours?

Answer: not something you should commit to.

His 1–2 hours comment is only realistic in a best-case architecture, such as:

required objects neatly isolated,
same platform/compatible setup,
read-only transition manageable,
datafiles copied efficiently,
very limited extra dependencies,
minimal remapping complications.

In your environment, with many tables mixed in shared tablespaces and many unrelated objects, 1–2 hours is not something you should promise.

Safer answer

“Transportable can be significantly faster than regular export/import when the tables are cleanly isolated in transportable tablespaces. But in our current layout, where the required tables are mixed with many other objects, it is not a straightforward 1–2 hour solution.”

Gangadhar suggested three things: test alternate Data Pump parameters, split the load into multiple jobs, and evaluate a transportable approach. All three are valid for testing. However, splitting the export/import and forcing direct path may provide only limited improvement if the real bottleneck is import-side IO and index creation. Transportable can be much faster, but only when the required tables are isolated in transportable tablespaces. In our current layout, since these tables are mixed with many other objects, that is not a quick 1–2 hour option without structural alignment.”


Hi Sunny,

I connected with Gangadhar to review a few additional options to improve the export/import performance.

We are planning to try the following:

* Split the export dump across two mount points to better utilize IO and reduce contention
* Test with `ACCESS_METHOD=DIRECT_PATH` to see if it gives any improvement
* Also looking at increasing memory on PSERP, which may help improve performance during import and parallel operations

I will run these tests and share the actual improvement numbers.

For the memory part, could you please review and approve increasing the memory on the PSERP server? Once approved, I can raise the request and proceed with testing.

Thanks,
Sai



This change is required to refresh Schema R in the PSERP database with the latest data from the SAPERP schema in VPRS (Cascade source staging database).

The refresh is necessary to:

Ensure data consistency and accuracy between source (VPRS) and target (PSERP)
Support ongoing Elevate testing and validation activities
Provide an updated dataset required for downstream processing and verification

Since the source database (VPRS) is operating in Active Data Guard (ADG) standby mode, a controlled pause of ADG replication is required to safely perform the export activity without impacting replication consistency.

This is a planned and controlled database activity, executed within a defined window, ensuring minimal risk and no direct impact to end users.


Backout Plan (Improved – High Rating Version)

Objective: Restore ADG replication and revert database to original state if any failure occurs during export/import or schema refresh.

Backout Steps:

Stop ongoing activity
Abort export/import jobs if running
Ensure no active sessions are impacting the database

Validate current database state

SELECT status, instance_name, database_role, open_mode 
FROM v$database, v$instance;

If database is in snapshot/read-write mode → revert to physical standby

SHUT IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE CONVERT TO PHYSICAL STANDBY;

Restart database in standby mode

SHUT IMMEDIATE;
STARTUP MOUNT;

Enable ADG replication

ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Validate ADG synchronization

SELECT THREAD#, SEQUENCE#, APPLIED 
FROM v$archived_log 
ORDER BY SEQUENCE# DESC;

Confirm database is back to normal state (READ ONLY WITH APPLY)

SELECT database_role, open_mode FROM v$database;
If Schema R refresh partially completed
Drop/revert impacted objects in Schema R (if required)
Retain previous stable state or re-initiate refresh in next window



Test Plan (Improved – High Rating Version)

Objective: Validate that Schema R refresh is successful and ADG replication is functioning correctly post activity.

Validation Steps:

Verify database status

SELECT status, instance_name, database_role, open_mode 
FROM v$database, v$instance;
Validate Schema R data refresh

Check object count:

SELECT COUNT(*) FROM dba_tables WHERE owner = 'SCHEMA_R';

Validate sample table data:

SELECT COUNT(*) FROM SCHEMA_R.<key_table>;
Compare with source (VPRS)
Validate row counts for critical tables between source and target

Check invalid objects

SELECT object_name, status 
FROM dba_objects 
WHERE owner = 'SCHEMA_R' AND status = 'INVALID';

Verify ADG replication resumed

SELECT PROCESS, STATUS FROM v$managed_standby;

Validate log apply

SELECT MAX(SEQUENCE#), APPLIED 
FROM v$archived_log 
GROUP BY APPLIED;
Basic performance sanity check
Run sample queries on Schema R
Ensure no latency or locking issues
Final confirmation
Database in expected mode
Data refreshed successfully
ADG in sync
