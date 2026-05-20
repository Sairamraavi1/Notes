Create a professional, management-level high-level document for Oracle DBA user access management, security controls, and operational procedures for a T-Mobile Elevate project environment.

Environment Details:

* Oracle production database name: VPR
* Oracle Active Data Guard standby database: VPRS
* VPRS is replicated from VPR using Oracle Active Data Guard (ADG)
* Due to ADG architecture, users must be created in VPR production database so they replicate automatically to VPRS
* Main concern from management/security teams is understanding why users are created in production even though access is intended mainly for VPRS standby validation/reporting activities

The document should clearly explain:

1. Environment architecture overview
2. Why user creation happens in VPR production
3. Difference between production creation and actual intended usage
4. Security controls implemented
5. Governance and approval process
6. DBA operational procedures
7. Monitoring and auditing capabilities
8. Risk mitigation and least privilege approach
9. Active Data Guard operational behavior
10. Access restrictions and protections

Include details for the following account types:

1. BODS Service Account

* Used for automated staging, extract, and transformation jobs
* Machine-to-machine communication only
* Restricted access to approved schemas/objects

2. PowerBI Dashboard Account

* Used for reporting/dashboard connectivity
* Read-only access only
* No DML or administrative privileges

3. Validation / DB Link Accounts

* Used for business validations and reconciliation activities
* Read-only access through DB link operations
* No write access allowed

The document should strongly emphasize:

* Principle of least privilege
* Read-only restrictions
* Controlled DBA provisioning
* Approval-based access
* Secure credential handling
* Auditing and monitoring
* Security tooling/governance
* Ability to revoke access immediately if required

Important clarification to include clearly:
“Although accounts are created in the VPR production database due to Oracle Active Data Guard replication architecture, the intended usage is strictly controlled and primarily focused on approved access to the VPRS standby environment for validation, reporting, and integration activities.”

The document should:

* Be easy for management and security teams to understand
* Be professional and well-structured
* Use simple but strong security/governance language
* Avoid overly deep technical Oracle terminology
* Include section headers and clean formatting
* Be around 2–4 pages in length
* Sound enterprise-level and audit-ready
* Clearly separate DBA responsibilities vs business/application usage

Also include:

* High-level operational workflow for user provisioning
* High-level security workflow
* Conclusion section summarizing governance and controls
