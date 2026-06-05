Hello Team,

We request your review and approval for the proposed DB link approach to support functional validation activities for the Mach3/Elevate testing efforts.

Overview:

* Source data resides in the VPRS environment (Cascade standby database)
* Validation and reporting activities are performed in the PSERP environment
* A DB link will be used to securely transfer only the required validation result sets between environments
* Users will access the approved DB link through dedicated controlled schemas, and credentials will be managed through CyberArk

Validation Process:

1. Validation queries are executed in VPRS against required source tables.
2. Required validation/result tables are generated for comparison and reporting.
3. Only required validation data is transferred from VPRS to dedicated validation schemas in PSERP through the approved DB link.
4. Functional users perform validation and reconciliation activities in PSERP.
5. No production transactional updates occur through the DB link; usage is limited to validation and reporting activities only.

Security & Controls:

* Dedicated user accounts will be created specifically for validation activities.
* Separate accounts will be maintained for BODS processing and validation activities to ensure segregation of duties.
* User creation requires Production DBA approval.
* Passwords will be managed through CyberArk.
* Imperva whitelisting will be completed before access is granted.
* Access will be restricted to only the required schemas and tables.
* Access will be limited strictly to validation activities.

Testing Results:

* Initial testing using the approved whitelisted account completed successfully.
* Mark completed testing without performance or connectivity issues.
* Timings were within the required validation window.

We request confirmation from the SOX team that this DB link approach is acceptable from a controls and compliance perspective.

Please let us know if any additional details or documentation are required.

Thanks,
Sai