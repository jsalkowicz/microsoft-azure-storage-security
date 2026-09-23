# Fictional Northstar claims scenario

A repair partner needs temporary read-only access to one approved claim summary, `claim_summary_CL-6201.txt`. A separate synthetic estimate, `claim_estimate_CL-6202.txt`, tests that the signed read URL cannot be moved to a different blob. These source files are in the local `claims-read/` directory, but in the live lab they were uploaded to the **existing Azure `claims` container**.

A customer needs to submit a synthetic text-only stand-in for a claim photo (`claim_photo_CL-6101.txt`), then a synthetic submission note (`customer_submission_note_CL-6101.txt`). These were placed in a **separate private Azure `claims-upload` container** using container-scoped Create + Write SAS tokens.

No actual photograph or real customer record is included. The names, claim IDs and facts are invented for security validation.
