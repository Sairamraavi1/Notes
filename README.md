Hi Uday,

As discussed with Josh and Sunny, we would like to request your approval to use Dev04/VE1 (or another suitable NPE environment if preferred) to perform a proof-of-concept test for the proposed validation approach.

The objective of this testing is to evaluate a push-based validation method where required data from the SAPERP schema is copied into dedicated validation schemas for Mark and Kevin, allowing them to perform validation activities within the NPE environment.

Proposed high-level testing steps:

Use the SAPERP schema in Dev04/VE1 as the source schema.
Create dedicated validation schemas for Mark and Kevin within the approved NPE environment.
Obtain the required access approvals for Mark and Kevin to perform validation activities in their respective schemas.
Identify the required tables and validation queries.
Configure the source and target schemas for testing.
Copy/push the required data from the SAPERP schema into the validation schemas.
Create any required indexes or supporting objects needed for validation.
Execute validation queries and capture performance metrics.
Document end-to-end timings, observations, and any dependencies or constraints.

The goal is to validate the feasibility of this approach and understand the overall execution effort and duration before considering its use for production-related validation activities.

Please let us know if Dev04/VE1 is an appropriate environment for this testing or if another NPE environment (such as P-Lab or Q-Lab) would be preferred.

Thank you for your review and approval.

Regards,
Sai
