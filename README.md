Physical standby is READ ONLY (with redo apply)
In this mode, no DDL is allowed
Creating a temp table is still DDL → so NOT allowed
If needed, we have only 2 options:
Create on PRIMARY → automatically replicates to standby
Convert standby to SNAPSHOT → then create tables (READ WRITE)

👉 Conclusion:

“We cannot create temp tables directly on standby unless we switch it to snapshot mode or pre-create them in primary.”

Slightly Detailed (if they ask follow-up)
1. Standby behavior
Physical standby runs in:
READ ONLY WITH APPLY
Redo is continuously applied from primary
2. Why temp tables are not allowed
Even Global Temporary Table (GTT) creation is:
A DDL operation
Standby blocks all DDL to maintain:
Redo consistency
Data Guard synchronization
3. What is allowed vs not allowed
❌ Not allowed
CREATE TABLE
CREATE GLOBAL TEMPORARY TABLE
Any structural change
✅ Allowed
SELECT queries
Using already existing objects
(If GTT exists) → session-level inserts
4. Available options
✅ Option 1: Create in PRIMARY (Recommended if static)
Create GTT in primary
Automatically available in standby
Safe, no disruption
✅ Option 2: Snapshot standby (what we already do)
Convert standby → READ WRITE
Then:
Create temp tables
Do T1/T2/T3 staging
After work:
Revert back to standby

👉 Impact:

Redo apply paused
Needs CR (which we already follow)
5. Recommendation (align with your project)
For BODS / staging:
Use snapshot standby window
For reusable structures:
Pre-create in primary
🔹 1-Line Executive Answer

“Temp tables can’t be created on standby in read-only mode since it blocks DDL; we either create them on primary or switch to snapshot standby for read-write.”

If you want, I can also give:
👉 exact error message you’ll get (good for confidence in call)
👉 or how to position this as optimization blocker + solution to Sunny (very useful for your discussion)
