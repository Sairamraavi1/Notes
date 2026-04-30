Issue:
We are facing timeouts and connection drops while querying large tables via DB Link (PSERP → VPRS).

Small queries are working fine
Large queries on big tables are failing or timing out

Error Observed:
Intermittent timeout / session disconnect during DB Link execution when accessing high-volume data.

Context:

Oracle 19c databases
Queries involve large SAP tables and joins
Minimal load during testing, still seeing the issue

Requirement:
We need to support high-volume validation queries within a limited 6–8 hour window, so stable performance is critical.

Ask:

Is DB Link recommended for large data queries in this scenario?
What tuning/configuration can help avoid timeouts?
Any better alternative approach suggested by Oracle?
