Yes. Since the actual requirement is VPRS (staging/Cascade) access, but VPRS is read-only and the accounts therefore need to be created on Production VPR and then become available on VPRS through replication, I would make that very explicit so the Production DBA team does not misunderstand the request.

Lavanya can use this directly for the approval email:

Subject: Approval Request – Read-Only Access to VPRS for Elevate Business Validation

Hi Godavari, Sunny and Neeta,

We are requesting your approval to provide the below users with read-only access to the VPRS (Cascade/Staging) environment to support Elevate business validation activities.

Request Details:

* Environment where access is required: VPRS – Cascade/Staging
* Source Database: ECC PRD / Production VPR
* Host: pttnasvpr00052
* Schema: SAPERP
* Access Type: Read-only
* Purpose: Elevate business validation activities
* Table Scope: Access will be restricted only to the approximately 160 approved SAP tables that are part of the Elevate export/import scope. Sai will provide the final approved table list.

Users requiring access:

* MTHOMAS356
* RSANTMY2
* RTHOMAS168
* DCL86664
* CMATHIS16

Important Note:
Although the users require access to the VPRS staging/Cascade environment, VPRS is maintained in read-only mode. Therefore, the required database users/privileges need to be created on the Production VPR database so that they are available on VPRS through the existing replication/Data Guard process. The requested access itself is read-only and limited to the approved SAP tables.

Please provide your email approval to proceed with this access request.

Once the approvals are received, we will raise the Compass request with the approval email attached. After completion of the required Compass approvals, the corresponding SC task will be routed to the Production DBA team to create/provision the requested users and read-only privileges.

Thanks,
Lavanya