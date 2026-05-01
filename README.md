Our requirement is:

Source database is Active Data Guard standby (read-only)
Target database is read-write (PSERP) where we need to load staging tables
We are currently trying to use DBLink for large data extraction, but it is not performing / failing

We need your guidance on:

Whether DBLink is recommended in this standby scenario for large data volumes
If not, what is the best alternative approach to:
Extract large data from standby
Load into staging tables in target DB

Specifically, please suggest supported approaches like:

Direct DBLink tuning (if possible)
Using Data Pump (expdp/impdp)
Any Oracle-supported replication / transfer method
Or any best practice for large data movement from standby

Our goal is to have a reliable and fast method during cutover window.
