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


You asked for causes, tuning recommendations, troubleshooting steps, and alternatives related to ORA-03150 / ORA-02063 and large-volume queries over database links.

Key observed causes and symptoms
ORA-03150: "end-of-file on communication channel for database link" often appears with ORA-02063 (preceding line from ), and may coincide with remote errors (e.g., ORA-07445) or connect/timeouts (ORA-12170, ORA-03113, ORA-12571).
Network/firewall behavior or improper on‑prem sqlnet.ora settings are commonly implicated.
Immediate configuration actions
Set SQLNET.EXPIRE_TIME to a non‑zero value on the on‑prem sqlnet.ora (example value used: 2).
Set sqlnet.send_timeout to a non‑zero value on the side receiving large data (a large value is an effective workaround).
If a firewall sits between servers, either disable SQLNet fixup / SQLNet packet inspection / SQL Fixup / SQL ALG / DPI rules or move the databases into the same netmask; if firewall rules cannot be changed, enable Dead Connection Detection (DCD) on the target server.
Parallelism and query tuning
Add a PARALLEL hint for the remote table within the distributed query so the remote database can parallelize its access to that object (used as a workaround when remote-side parallelism helps).
Troubleshooting notes
Check for remote database trace/alert entries (e.g., ORA-07445) when ORA-03150 appears locally.
Intermittent ORA-03113 / ORA-12571 can be caused by firewall packet inspection or network timeouts.
Not addressed here / I do not have relevant information to answer
I do not have relevant information to answer questions in this context about whether DB links are recommended for large data movement, tuning specifics for RECEIVE_TIMEOUT, SDU/TDU, TCP keepalive, HS/gateway configuration, detailed distributed query performance tuning, ORA-600 during large fetches, best-practice alternatives (GoldenGate, Data Guard snapshot standby, materialized views fast refresh, Data Pump, DBMS_SCHEDULER with staging), fetch_size/array size tuning, distributed_transaction usage, or FAL timeout settings; additional details can be provided if you supply specific areas to investigate.

Sources:
[ADB-S] Querying Via DB Link From ADB-S To On-Prem In Intervals Fails With "ORA-03150: end-of-file on communication channel for database link"
Query Using DBLink Returns ORA-3150 ORA-2063 on Local DB & ORA-7445 [ttcfour()+413] on Remote DB
RDBPROD: A Query via DB Link from Oracle to Rdb Fails Intermittently with ORA-02068 and ORA-03113
Error "ORA-03150: end-of-file on communication channel for database link" in ETL Step 16 W_ROLE_LIMIT_FS.sql and Process Fails
DBLINK: ORA-03113, ORA-12571 Issuing a Select Over a Database Link Through a Firewall
Bug 18841764 - Network related error like ORA-12592 or ORA-3137 or ORA-3106 may be signaled
InForm Data Load Job Fails with 'ORA-03150: end-of-file on communication channel for database link' 
