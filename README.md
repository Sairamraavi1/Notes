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



Post Backout Validation:
- Confirm database_role = PHYSICAL STANDBY
- Confirm open_mode = READ ONLY WITH APPLY
- Verify log apply is active and no significant lag
- Validate database status is stable

- Communication Plan:
- Provide updates every 15–30 minutes until recovery completion

- Implemantion plan

- Pre-Execution Validation:
- Verify sufficient disk space for export dump files
- Confirm ADG is fully in sync before pause
- Validate required directory objects and permissions
- Ensure no blocking sessions on source/target databases

- Add this AFTER ADG Pause step
Monitoring During Execution:
- Monitor datapump jobs:
  SELECT * FROM dba_datapump_jobs;

- Monitor active sessions and wait events
- Track CPU, memory, and I/O utilization

- Add this BETWEEN Export and Import steps
Checkpoint Validation:
- Confirm export completed successfully with no errors
- Validate dump file availability and size

- Add this BEFORE ADG Resume step
Pre-Resume Validation:
- Ensure import completed successfully
- Validate Schema R object count and key tables
- Check for invalid objects

- Add this at END of Implementation Plan
Post-Execution Validation:
- Resume ADG and confirm log apply
- Verify database in READ ONLY WITH APPLY mode
- Validate no replication lag
- Capture logs (expdp/impdp/alert logs) for audit reference


Add this at the VERY TOP (before Objective)
Backout Trigger Conditions:
- Export or Import job failure
- Schema R refresh incomplete or failed
- ADG replication not resuming or high lag observed
- Data validation mismatch between source (VPRS) and target (PSERP)
👉 2. Add this AFTER Objective (before Backout Steps)
Backout Ownership:
- Primary: DBA Team
- Support: Infrastructure Team (if required)

Expected Recovery Time (RTO):
- ADG restoration and database normalization within 30–60 minutes
👉 3. Add this at the END (after your last SQL step)
Post Backout Validation:
- Confirm database_role = PHYSICAL STANDBY
- Confirm open_mode = READ ONLY WITH APPLY
- Verify log apply is active and no significant lag
- Validate database status is stable

Communication Plan:
- Notify stakeholders immediately upon backout initiation
- Provide updates every 15–30 minutes until recovery completion
