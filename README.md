Hi Sunny,

As discussed, I reviewed the logging feasibility from the database perspective and wanted to summarize the current setup, limitations, and possible options.

---

**1. Regular Audit Log Collection Process (Standard Approach)**
In a typical setup where the database is in READ-WRITE mode:

* We can enable database auditing/logging
* We can track user activity, table access, and privileged operations
* Logs are generated locally and can be forwarded to downstream systems (e.g., Splunk)

This is the standard approach where DBA has full control over audit log collection.

---

**2. VPRS / Staging Database (Current Setup)**

* VPRS is a replicated copy of Polaris production
* It operates in READ-ONLY mode
* It is under continuous replication

**Limitation:**

* We cannot enable or generate database-level audit logs locally in VPRS
* No independent logging can be introduced while it remains read-only

Even if auditing is enabled at the Polaris production level:

* Logs will capture only production activity
* These logs may get replicated to VPRS

However:

* This will NOT capture activity happening in VPRS
* It will NOT track BOT/BODS access during staging execution
* It reflects only production usage, not staging usage

So this approach does not meet the requirement of tracking VPRS activity.

---

**3. Read-Write Window (During BOT Execution)**
During controlled execution windows:

* The database may be temporarily switched to READ-WRITE mode
* BOT/BODS jobs run during this period

In this window, we can capture:

* Service/BOT user access
* Tables accessed (e.g., ADRC, LFA1, etc.)
* Timestamps

---

**4. Available Option (From DB Side)**

**Option: Database Auditing during READ-WRITE window**

* Enable targeted auditing for required/sensitive tables
* Capture BOT/service account activity
* Forward logs to Splunk

**Requirements:**

* READ-WRITE window availability
* List of sensitive tables
* List of BOT/service users
* Location to generate/store logs
* Splunk integration

---

Please let me know if you would like us to proceed with this approach or explore alternatives from SAP/application side as well.

Thanks,
Sai




