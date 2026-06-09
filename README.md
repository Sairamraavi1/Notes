Create a professional enterprise architecture infographic/technical workflow diagram for “DB Link Access Provisioning Approach – VPRS Validation”.

The diagram should visually explain:

* When the approach works
* When it does not work
* Snapshot mode dependency
* Temporary vs persistent users/objects
* Best provisioning strategy for validation users

Style:

* Clean enterprise architecture style
* Professional Oracle/SAP/DBA presentation look
* Use arrows, lifecycle flow, icons, and status indicators
* White background with blue/green/orange/red sections
* Clear separation between “Read Only” and “Read Write Snapshot” modes
* Use database cylinder icons, lock icons, user icons, rollback icons, sync arrows

Title:
“DB LINK ACCESS PROVISIONING APPROACH – VPRS VALIDATION”

Subtitle:
“When It Works, When It Does Not, and Recommended Approach”

Main workflow should contain 4 phases:

1. NORMAL STATE (READ ONLY)

* VPRS in Physical Standby Mode
* Database is Read Only
* DB Link creation NOT allowed
* Cannot create users/schemas
* Cannot create objects/tables
* Validation activities will not work
* Show red X indicators

2. SNAPSHOT MODE (READ WRITE)

* VPRS converted to Snapshot Database
* Database becomes Read Write
* DB Link creation works
* Can create temporary users/schemas
* Can create validation tables/objects
* Validation activities work
* DBA can create additional temporary users if required
* Use green check marks

3. CONVERT BACK TO READ ONLY

* Snapshot converted back to Physical Standby
* Resynchronization with primary database
* All temporary changes rolled back
* DB Links removed
* Temporary users removed
* Temporary schemas/objects removed

4. AFTER RESYNC

* Database back to original Read Only state
* No temporary users exist
* No DB Links exist
* System returns to original standby state

Include a “KEY ROLES” side panel:

* Mark & Kevin → Need access for validation activities
* Service Account (Whitelisted) → Used for DB Link connectivity
* Mark NTID / Schema User → Used for schema validation activities
* DBA → Creates DB Links and temporary users during snapshot window

Include an “ACCESS PROVISIONING STRATEGY” table:
Columns:

* Purpose
* Recommended User
* Whitelisting Required?
* When Used
* Persists After Read Only?

Rows:

1. DB Link Connectivity

   * Use Whitelisted Service Account
   * Yes whitelist required
   * Only during Snapshot Mode
   * Does NOT persist after resync

2. Schema Validation Activities

   * Use Mark NTID or temporary schema
   * Optional whitelist depending on requirement
   * During Snapshot Mode
   * Does NOT persist after resync

3. Additional Temporary Users

   * Created by DBA during snapshot window
   * No permanent whitelist required
   * Temporary only
   * Automatically removed after rollback/resync

Add a highlighted “BEST PRACTICE APPROACH” section:

* Use whitelisted service account for DB Link connectivity
* Use Mark NTID or temporary schemas only during snapshot mode
* Create any extra validation users during approved change window
* After converting back to read-only standby, all temporary users, DB links, and objects are automatically rolled back

Add a “WHAT WORKS / DOES NOT WORK” comparison box:

WORKS ONLY IN SNAPSHOT MODE:
✔ Create DB Links
✔ Create Users/Schemas
✔ Create Validation Objects
✔ Run Validation Activities

DOES NOT WORK IN READ ONLY MODE:
✘ Cannot create DB Links
✘ Cannot create Users/Schemas
✘ Cannot create Objects
✘ Validation activities fail

Design Requirements:

* Modern enterprise infographic
* Clear readable typography
* Executive presentation quality
* Use process arrows and lifecycle flow
* Include rollback/synchronization visual indicators
* Make the flow easy for CAB, CyberArk, DBA, and Validation teams to understand quickly
