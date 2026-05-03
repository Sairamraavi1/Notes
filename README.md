Hi Team,

We are currently facing an issue with the DB link connectivity between the VPRS (Cascade) and PSERP databases and would appreciate your assistance.

**Issue Summary:**

* DB link is getting established successfully, and basic connectivity checks are working.
* However, for certain queries, we are encountering errors, while the same queries work in other scenarios.
* We have validated the DB link configuration and query syntax with internal teams (DLM and DBAs), and no issues were identified from that perspective.

**Troubleshooting Done:**

* Verified DB link configuration and credentials.
* Tested queries directly and via DB link – inconsistent behavior observed.
* Paused ADG replication temporarily and re-tested → issue still persists (so ADG does not seem to be the cause).
* Checked with multiple teams (DLM / Production DBAs), but no clear root cause identified so far.

**Ask:**
Could you please help review this issue and provide guidance on:

* Possible causes for partial DB link query failures
* Any known limitations or considerations when querying across these environments
* Recommended approach to stabilize DB link usage between Cascade and PSERP

We already have an Oracle SR open and are working in parallel with them, but any insights or suggestions from this team would be very helpful to unblock us.

Please let me know if you need any additional details, logs, or error messages from our side.

Thanks in advance for your support.

Regards,
Sai
