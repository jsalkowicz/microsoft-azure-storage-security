# PUBLIC / HOLD Review

Run this check before publishing or updating the repository.

## PUBLIC

The repository can be public when all of the following are true:

- [ ] Claims about the work are accurate.
- [ ] The README clearly describes this as a training / hands-on learning environment.
- [ ] No Storage account keys are present.
- [ ] No SAS token or full SAS URL is present.
- [ ] No passwords, access tokens, or client secrets are present.
- [ ] Subscription IDs and tenant/account identifiers are removed unless genuinely necessary.
- [ ] Screenshots are cropped and sanitized.
- [ ] No real customer, employee, or company-sensitive data is present.
- [ ] Screenshot links render correctly.
- [ ] Commands do not contain live credentials.
- [ ] Actual test results are documented.
- [ ] Failed tests and troubleshooting are described accurately.
- [ ] The work can be explained in an interview.

## HOLD

Keep the repository private if:

- a live secret is visible
- screenshots are not sanitized
- the lab has not actually been performed but the README claims it was completed
- results are copied from expected output rather than observed
- the repository contains temporary IDs, URLs, or tokens that should not be public

## Human Pass

Before publication, read the README out loud and ask:

> Could I explain every statement here to an interviewer based on work I personally performed?

If not, revise it.
