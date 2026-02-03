Current cutover process includes a full Data Pump export/import of 160 tables, which takes ~3 days.
Target cutover window is 4–8 hours.

We are evaluating an architectural optimization to remove this bottleneck while maintaining compliance and validation integrity.

🏗 Current Architecture (Mock 3 Model)

SAP ECC Production → DR (Titan) via Active Data Guard (ADG)

DR → Source Staging DB via ADG (real-time replication)

Pause ADG

Export 160 tables from Source Staging

Import into Schema R

Run USGCI masking script in Schema R

BODS extracts from Schema R → transforms → loads into Schema C

Validation performed against Schema R

⚠️ Pain Point

The export/import step consumes ~3 days and is not feasible for a 4–8 hour cutover window.

🚀 Proposed Optimization

Eliminate the Data Pump step and:

Allow BODS to extract directly from Source Staging DB

Pull only required tables (Master vs Transactional subsets)

Load into Schema R

Run masking in Schema R

Continue validation against Schema R

This reduces unnecessary movement of unused tables and removes the 3-day bottleneck.

🛡 Compliance & Controls Position
Already Covered (SOX Controlled)

Production → DR via ADG

DR → Source Staging via ADG

These rely on existing replication controls.

Current Gap

Manual Data Pump movement into Schema R does not automatically inherit those controls.

Evidence capture for Data Pump is not formally structured today.

With Proposed Model

Control boundaries become:

ADG replication (controlled)

BODS ingestion into Schema R

Masking execution in Schema R

Validation from Schema R downstream

✔ Certification can be performed once at platform level
✔ Workstreams should not need upstream re-validation

🔐 Masking Position

Current method: USGCI masking script runs in Schema R

Cyber concern was logging behavior; no sensitive data logs confirmed.

If corporate mandates enterprise masking tool (e.g., DataShield) before cutover → adopt it.

Otherwise, existing script remains operationally valid.

Schema R remains validation baseline to avoid access and compliance complexity in Source Staging.

📉 Optimization Opportunity

Only subset tables required for Master loads.

Separate subset for Transactional loads.

Possible row-level filtering (if documented and approved).

This reduces volume and cutover execution time significantly.

⏱ Required Next Steps

Before cutover decision:

Confirm BODS connectivity to Source Staging DB.

Identify exact table subset actually used.

Validate extraction performance under load.

Document BODS ingestion control.

Finalize masking execution model.

Timebox architecture validation testing before cutover.

🧠 Executive Conclusion

The three-day delay is caused by Data Pump export/import.
Direct extraction from the ADG-controlled staging database eliminates this bottleneck while preserving compliance boundaries.

Schema R remains the trusted validation anchor.
Final decision requires performance testing and compliance sign-off within the next few weeks.
