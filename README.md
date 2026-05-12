Proposed approach discussed:

1. Temporarily pause ADG and convert VPRS/Cascade DB to read-write mode.
2. Create and test the reverse DB link approach (VPRS → PSERP).
3. Execute the validation queries and capture performance metrics.
4. Re-enable ADG after testing is completed.

This approach helps avoid requesting direct production SAP DB access for individual users while still allowing us to validate whether the reverse DB link improves overall query performance during the cutover validation window.

Next step is to review this approach with Pratap, Sunny, and Neeta and obtain approval before scheduling the CR/testing activity.
