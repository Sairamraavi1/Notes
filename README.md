Requesting creation of a read-only service account for DB link access as part of Elevate business validation.

We require a dedicated service account to enable controlled access from PSERP to the VPRS (Cascade) source database via DB link.

Details:
Database Server Name: ECC PRD
Database Name: pttnasvpr00052 (VPRS)
Target Schema: SAPERP (Source system)

Proposed Service Account Name: SVC_PRD_ELEVATE_DBLINK_RO

Access Required:
- Read-only access to required SAP tables in SAPERP schema
- No DML permissions (INSERT/UPDATE/DELETE)
- Access will be used strictly via DB link from PSERP

Purpose:
This service account will be used by the Elevate business validation team to query SAP ECC data through DB link for reconciliation, reporting, and validation activities.

Notes:
- This account will be used as the underlying credential for DB links (not for direct user access)
- No direct login access is required for business users
- Access should comply with read-only and security standards

Kindly create the service account and grant appropriate read-only privileges.


SVC_PRD_ELEVATE_DBLINK_RO
