Summary – Scratch-pad tables on Standby DB

I explored whether we could create local scratch-pad / staging tables on the Oracle 19c physical standby to support intermediate processing and optimization.

I tested creating:

A local tablespace on standby

Private / temporary tables using Oracle 19c features

However, the standby database is opened in read-only mode, and Oracle does not allow any DDL operations in this state. Even private or temporary tables require data-dictionary updates, which are blocked on a physical standby by design.

Because of this:

Scratch-pad or staging tables cannot be created on the standby

This is an Oracle limitation, not a configuration or access issue

Way forward

On standby, we can only use SELECT-based logic (CTEs, inline views, subqueries).

Any table-based staging or scratch-pad processing will need to remain on the primary database or another writable environment.
