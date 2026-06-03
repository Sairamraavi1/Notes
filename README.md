Hi Team,

As discussed during today's call, we reviewed the current VPRS to PSERP DB link testing results and identified a potential security-related dependency that requires further validation.

Summary of findings:

* PSERP to VPRS DB link testing is working successfully.
* VPRS to PSERP DB link connectivity is established; however, query execution fails with ORA-03113 (End-of-file on communication channel).
* Oracle versions, TNS configuration, and SQLNET configuration have been reviewed and do not currently indicate a connectivity issue.

Current working hypothesis:

Based on previous experience with newly created users in VPRS, we believe the issue may be related to additional security controls such as CyberArk onboarding and/or Imperva whitelisting requirements.

Next Steps:

1. Obtain the required CR approval for a testing window.
2. Retest the VPRS to PSERP DB link using the existing SNF_ECC_BODS_USER account, which is already known and enabled within the environment.
3. Compare the results against the newly created validation user.
4. If the SNF_ECC_BODS_USER test succeeds, proceed with applying the same security onboarding and approval process to Mark's user account.
5. If the issue persists with the SNF_ECC_BODS_USER account, continue investigating alternative Oracle/DB link related causes.

This testing will help determine whether the issue is related to security onboarding or the DB link implementation itself.

Thanks,
Sai
