# VPRS to PSERP DB Link Testing Summary

## Objective

Validate the DB link approach for business validation activities between VPRS and PSERP and determine whether the proposed methodology can support Mark's validation testing requirements.

## Activities Performed

### User Creation

Created dedicated testing users for the validation activity:

* Mark Tambling NTID user
* PSERP_CE user

These users were created to support validation testing and object ownership requirements.

The users were provided:

* Required object privileges
* Tablespace allocation
* Quota assignment
* Ability to create validation objects as required

### DB Link Configuration

Configured and tested DB links between:

* PSERP → VPRS
* VPRS → PSERP

### Oracle Configuration Review

Reviewed and validated:

* tnsnames.ora
* sqlnet.ora
* DB link definitions
* Oracle version compatibility

Verified all environments are running Oracle 19.23.

## Test Results

### PSERP → VPRS

Result: Successful

Observations:

* DB link connectivity successful
* Queries executed successfully
* No communication channel errors observed

### VPRS → PSERP

Result: Failed

Observed Error:

* ORA-03113: End-of-file on communication channel

Observations:

* DB link connectivity established successfully
* User authentication successful
* Error occurs during query execution

## Analysis

Since:

* Connectivity exists
* Authentication succeeds
* PSERP → VPRS works successfully
* Oracle versions are aligned
* TNS and SQLNET configurations were reviewed

The issue does not currently appear to be a basic Oracle connectivity issue.

## Current Hypothesis

VPRS operates with additional security controls, including:

* CyberArk
* Imperva

Historically, similar behavior was observed with the SNF_ECC_BODS_USER account where database access required additional security onboarding and whitelisting before functioning correctly in VPRS.

The current hypothesis is that the newly created testing users may require:

1. CyberArk onboarding/enablement
2. Imperva whitelisting/approval

before they can be fully utilized for this testing scenario.

## Why a New User Was Created

A dedicated user was intentionally created rather than immediately reusing the SNF_ECC_BODS_USER account because:

* Separation of duties
* Cleaner audit trail
* Ability to allocate tablespace and quotas
* Ability to create and maintain validation objects
* Avoid impact to the existing BODS service account

This follows standard Oracle security and administration practices.

## Next Steps

1. Validate whether the new users require CyberArk onboarding.
2. Validate whether Imperva whitelisting is required.
3. Retest using the existing SNF_ECC_BODS_USER account as a comparison.
4. If the SNF account succeeds, confirm the issue is related to user onboarding/security controls rather than Oracle DB link configuration.
5. Complete onboarding for the new users and retest.


Hi Team,

Yesterday we completed DB link testing between VPRS and PSERP using the newly created validation users.

Summary of results:

* PSERP to VPRS DB link testing completed successfully.
* VPRS to PSERP connectivity was established successfully; however, query execution failed with ORA-03113 (End-of-file on communication channel).
* We reviewed the DB link configuration, tnsnames.ora, sqlnet.ora settings, and Oracle version compatibility. All environments are currently running Oracle 19.23.

Based on the testing results, the issue does not appear to be a basic Oracle connectivity or DB link configuration problem, as the connection is successfully established and the reverse direction (PSERP to VPRS) works as expected.

Our current assessment is that the newly created validation users may require additional onboarding through CyberArk and/or Imperva before they can be fully utilized within the VPRS environment. We have previously observed similar behavior with the SNF_ECC_BODS_USER account, which required additional security enablement.

As a next step, we plan to:

* Validate CyberArk onboarding requirements for the newly created users.
* Validate Imperva whitelisting requirements.
* Perform comparative testing using the existing SNF_ECC_BODS_USER account.
* Retest after any required security onboarding activities are completed.

We will provide additional updates as testing progresses.

Thanks,
Sai

