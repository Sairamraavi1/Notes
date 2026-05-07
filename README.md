Hi Team,

As discussed, below is the clear flow and protection approach for Validation users and BODS access in the Polaris/Cascade environment.

Environment Flow:

Polaris Production
↓
DR Database (Read-Only)
↓
Cascade Database (Read-Only)
↓
Validation Team / BODS Application

1. Validation Team Access (DB Link Users)

Validation users will access data only through Oracle DB links.

Example:
SELECT * FROM table_name@MARK_DBLINK;

Key Points:

* Users will know only the DB link name.
* No username/password will be shared with business users.
* No Polaris hostname, service name, or TNS details will be shared.
* Actual credentials remain internally stored and managed by DBA team.

Because of this:

* Validation users cannot directly connect to Polaris production.
* They cannot independently use SQL Developer/DBeaver against Polaris.
* Their access remains controlled and read-only through approved DB link paths only.

2. BODS Application Access

For BODS connectivity, the application requires credentials which are managed securely through CyberArk.

Current concern:

* Since the same account originates in Polaris production and propagates downstream, there is concern that:

  * Wrong hostname usage
  * Cached connections
  * Misconfiguration
  * Accidental connection attempts

could unintentionally attempt connection to Polaris production.

3. Current Technical Limitation

Listener-level blocking is limited because:

* Polaris and VPRS/Cascade share similar listener and port architecture.
* Both environments reside on the same server infrastructure.

Because of this, listener-level separation alone is not sufficient.

4. Recommended Protection Controls

The strongest protection approach is:

* Restrict access only from approved BODS application servers/IPs.
* Security tools should validate source machine/IP before allowing connection.
* Enable Imperva, Splunk, and Oracle auditing for monitoring.
* Track login attempts, failed logins, and account lock events.
* Keep all accounts strictly read-only with only:

  * CREATE SESSION
  * SELECT privileges

Additional recommendation:

* Oracle logon trigger validation can be implemented to allow only approved machines/programs.

5. Final Understanding

Validation Users:

* Do not know usernames/passwords.
* Only use DB link names.
* Do not have direct production access capability.

BODS Users:

* Credentials exist for application connectivity through CyberArk.
* Access should be restricted only through approved BODS servers and monitored continuously.

Overall, the complete design remains read-only, monitored, and controlled to protect Polaris production.

Thanks,
Sai
