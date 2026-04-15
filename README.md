Method	Description	Parameters Used	Timing	Observations	Improvement Scope
Method 1	Standard Export (VPRS)	PARALLEL=8	~4 hrs 39 mins	Stable execution, no major gain	Limited improvement
Method 2	Export with Higher Parallelism	PARALLEL=16	Tested during Schema R → S copy	No significant improvement	Resource bottleneck (CPU/IO)
Method 3	Export to 2 Mount Points	Multiple dump locations	Same as above	No major performance difference	IO not main bottleneck
Method 4	Direct Path Export	ACCESS_METHOD=DIRECT_PATH	In progress/testing	Expected faster throughput	Needs validation
Method 5	Import - Table Data Only	EXCLUDE=INDEX, STATISTICS	~18 hours (data only)	Better control on failures	Recommended approach
Method 6	Separate Index Creation	Post import job	+5 hours approx	Reduces risk & improves control	Optimal approach
Method 7	Parallel Import (Split Jobs)	PARALLEL=8	Improved slightly	Better than single job	Can be optimized further
Method 8	Big Tables Separate Jobs	Selected large tables separately	Reduced bottleneck	Avoids long tail delay	Good improvement
Method 9	Disable Archive Logging	TRANSFORM=DISABLE_ARCHIVE_LOGGING:Y	Reduced redo overhead	Some performance gain	Recommended
Method 10	DB Link Approach	Not tested	-	Expected minimal gain	Needs validation

=======================================================================================================================================================

Final Excel Structure (Recommended Format)
🔷 Sheet 1: Executive Summary (Top View for Sunny)
Area	Current State	Observation	Impact	Recommendation
Export Performance	~4 hrs 39 mins	Stable, no major bottleneck	Low concern	No major changes required
Import Performance	~18+ hours	Major bottleneck	High impact	Needs optimization
Parallelism	8 vs 16 tested	No significant improvement	Resource limitation	Increase memory
IO (Mount Points)	1 vs 2 mount points	No improvement	Not bottleneck	No action needed
Large Tables	Processed at end	Causing delay	High impact	Separate execution
Index Creation	Done separately	Better control	Medium	Recommended approach
Archive Logging	Disabled	Reduced redo overhead	Medium	Keep using
BODS Parallel Run	Not tested	Expected contention	High risk	Needs coordination

Detailed Testing Methods
Method ID	Method Description	Parameters Used	Execution Approach	Timing	Result	Conclusion
M1	Export (VPRS)	PARALLEL=8	Standard export	4h 39m	Stable	Baseline
M2	Export High Parallel	PARALLEL=16	Schema R → S copy	Similar timing	No gain	Resource bound
M3	Multi Mount Export	2 mount points	Split dump files	No change	No gain	IO not issue
M4	Direct Path Export	ACCESS_METHOD=DIRECT_PATH	Testing	TBD	Expected gain	Validate
M5	Import (Data Only)	EXCLUDE=INDEX, STATISTICS	Single job	~18 hrs	Controlled	Good approach
M6	Index Separate Job	Post import	Parallel index build	+5 hrs	Flexible	Recommended
M7	Parallel Import	PARALLEL=8	Multiple jobs	Slight gain	Limited	Improve further
M8	Large Tables Split	Separate jobs	Big tables isolated	Improved	Reduced delay	Best approach
M9	Disable Archive Logging	TRANSFORM=DISABLE_ARCHIVE_LOGGING:Y	Applied	Faster	Reduced redo	Keep using
M10	DB Link	Not tested	Planned	-	TBD	Needs validation

Bottleneck Analysis
Bottleneck Area	Description	Severity	Evidence	Action Required
Memory	Limited RAM impacting parallelism	High	No improvement with parallel 16	Increase memory
Large Tables	Processed last causing delay	High	Long tail in import	Split execution
IO Throughput	Disk performance	Low	No change with 2 mounts	No action
Parallel Jobs	Resource contention	Medium	Limited gain	Tune based on memory
Concurrent BODS	Shared resources	High	Not tested but expected	Plan execution window

Improvement Plan
Priority	Improvement	Expected Benefit	Effort	Dependency
High	Increase Memory (RAM)	20–30% improvement	Medium	Infra approval
High	Pre-create Datafiles	Faster import	Low	DBA
High	Split Large Tables	Reduce delay	Low	DBA
Medium	Direct Path Validation	Faster export	Low	Testing
Medium	DB Link Testing	Avoid exp/imp	Medium	Access
Medium	Parallel Optimization	Better utilization	Medium	Memory
Low	Transportable Tablespaces	Major redesign	High	Feasibility


Infrastructure Requirements
Requirement	Details	Purpose
Storage (Export)	10 TB	Multiple testing cycles
Storage (Import)	15 TB	Parallel import + index
Memory Increase	Align with DR (~700GB)	Improve performance
Oracle SR	Raise with Oracle	Expert validation


Method 1 – VPRS → PSERP_R (Full Refresh)
Method	Description	Execution Approach	Parameters Used	Timing	Result	Key Observation
M1	Full refresh from Cascade DB to PSERP	Export from VPRS (SAPERP) → dump files → Import into PSERP_R → Index creation post import	Disable archive logging during import	Export: 3 hrs
Import: 16 hrs
Index: 23 hrs	Completed successfully	Import & index creation are major bottlenecks
🔹 Method 2 – PSERP_R → PSERP_S (Schema Copy – High Parallel Export)
Method	Description	Execution Approach	Parameters Used	Timing	Result	Key Observation
M2	Internal schema copy within PSERP	Export from PSERP_R using high parallel → dump files → Import into PSERP_S using parallel → Full import (data + indexes together)	Export: PARALLEL=16
Import: PARALLEL=8	Export: 4h 39m
Import: 18 hrs	Completed successfully	Higher parallel did not improve performance significantly
🔹 Method 3 – PSERP_R → PSERP_M (Schema Copy – Standard Parallel)
Method	Description	Execution Approach	Parameters Used	Timing	Result	Key Observation
M3	Internal schema copy (alternate run)	Export from PSERP_R → dump files → Import into PSERP_M → Index creation post import	PARALLEL=8	Export: 5 hrs
Import: 20 hrs
Index: 24 hrs	Completed successfully	Slower than Method 2, shows resource constraint


Hi Sunny,

As discussed, I have completed multiple rounds of testing to evaluate the performance of the export/import process and to identify optimization opportunities.

**Summary of what has been tested:**

* Export with parallel 8 and 16
* Export across multiple mount points
* Import split into table data and index creation
* Use of disable archive logging to reduce redo
* Handling large tables separately
* Parallel execution for better throughput

**Key observations:**

* Increasing parallelism did not result in significant improvement, indicating possible resource constraints.
* Export to multiple mount points did not show noticeable gains, suggesting IO is not the primary bottleneck.
* Import phase remains the major bottleneck (~18 hours), especially due to large tables being processed at the end.
* Splitting table and index jobs provided better control and reduced failure impact.

**Enhancements identified:**

* Increasing memory is expected to improve performance significantly (based on observed differences between environments).
* Pre-creating datafiles/tablespaces will help avoid runtime delays.
* Running large tables separately helps reduce long tail delays.
* Direct path export is being tested further.
* DB link approach will be tested, though expected impact is minimal.

**Challenges / Risks:**

* Concurrent BODS activity during import may further impact performance.
* Limited storage is restricting multiple test iterations.

**Next steps / Requirements:**

* Additional storage: ~10TB for export and ~15TB for import testing
* Memory increase for PSERP (to align closer with DR capacity)
* Raising an Oracle SR to validate if further optimizations are possible

Please let me know if we can proceed with the above infrastructure changes so I can perform a complete end-to-end performance validation and provide more accurate timelines.

Thanks,
Sai

