Hi Team,

I wanted to share an update on the optimization efforts performed for the export/import activity between Schema R→S and Schema C→E.

As part of the optimization:

Archive logging was disabled during the activity window, which helped reduce redo generation and provided some improvement in throughput.
Parallelism (parallel=16) was tested for the export process; however, we did not observe a significant improvement compared to the previous run.

From the analysis, although the server has substantial overall capacity, most of the memory resources are currently allocated to the cascade database (VPRS). As a result, the ability for parallel operations to scale further remained limited during this run.

Summary (Key Points):

Archive log disable → Provided some performance improvement
Parallel=16 → No noticeable improvement observed
Observation → Parallel execution did not scale further compared to previous runs
DR Comparison → DR environment has ~700 GB RAM; performing a similar test there is expected to provide better performance
Action → Validate performance in DR environment / optimize for better scaling
Next Step → Re-test in next run and compare performance metrics
