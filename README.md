Hi Rahul,

I reviewed the violations listed in the Excel. These findings are tied to log4j 1.2.x components packaged within the SAP BODS installation paths on the application servers (CVE-2021-4104 – JMSAppender RCE).

This is outside the database scope and will require remediation from the BODS / SAP BusinessObjects application or Windows server team, typically via SAP-approved patches or BODS upgrade to a supported version where log4j 1.x is removed.
