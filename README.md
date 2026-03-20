Hi Sunny,

Got it, thanks for the clarification — we will plan to enable standard logging during the READ-WRITE window when BODS jobs run.

I just had a couple of quick clarifications to make sure we align correctly before setting this up:

Scope of logging
Should the logging be:

At a schema level (all activity under the schema), or

Limited to specific USGCI-sensitive tables (e.g., ADRC, LFA1, etc.)

My assumption is that focusing on USGCI tables may be sufficient and will also help reduce noise, but wanted to confirm.

Users to capture
Should we:

Capture only BOT/service accounts, or

Include any manual/DBA access as well during that window

Level of detail expected
Please confirm if capturing below is sufficient:

Username / service ID

Table accessed

Timestamp

Any exclusions
Do we need to explicitly exclude any standard/system users (e.g., SAP/system IDs) to avoid unnecessary noise?

Output / Integration

Is forwarding the generated logs to Splunk sufficient for Cyber review?

Or is there any additional format/report expected?

Please let me know if I’m missing anything important or if there are any specific requirements from Cyber that we should incorporate.

Thanks,
Sai
