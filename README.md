Subject: DB Link Testing Update – Successful Validation and Next Steps

Hello @Godavari, Sunny, @Abrego, Joshua,

Providing an update on last night’s DB link testing activities.

The testing was completed successfully using the existing user account SNF_ECC_BODS_USER. Mark was able to complete the validation testing successfully, and no performance or connectivity issues were observed during the execution. Based on the results and timings, the approach should work within the required testing window from a performance perspective.

This confirms that the earlier issue has been resolved and also helps narrow down the root cause to the user/account setup and required whitelisting process. The testing validated that the DB link works successfully when the user is properly configured and whitelisted.

For last night’s testing, we temporarily used the existing BODS account (SNF_ECC_BODS_USER) for validation purposes. However, moving forward, it would be better to have separate dedicated users for Mark’s validation activities, since the required grants and access for BODS processing and validation testing are different.

Next steps required:

* Create two dedicated users for the validation activities
* Obtain the required approvals for user creation
* Complete CyberArk approval process
* Complete Imperva whitelisting for the new users

Once the above activities are completed, Mark should be able to independently perform the validation testing going forward without dependency on the BODS user account.

Mark will share the detailed testing results and timing information separately.

Thanks,
Sai