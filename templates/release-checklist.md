# Release checklist

Copy this into the pull request or release ticket for every production release.

**Item:** ______________________  **Workspace:** ______________________
**Author:** ______________________  **Reviewer:** ______________________
**Release date:** ______________________  **Rollback version:** ______________________

## Before Test

- [ ] Change described in plain language (what changed and why)
- [ ] Peer review complete (someone other than the author)
- [ ] Best Practice Analyzer scan run; exceptions noted
- [ ] No duplicate definitions of certified metrics introduced

## Before Production

- [ ] UAT sign-off from the business owner
- [ ] Refresh succeeded in Test; row counts and key totals match the source
- [ ] RLS tested with "View as" (at least two personas)
- [ ] Sensitivity label and endorsement status confirmed
- [ ] Deployment rules reviewed (data source and parameters point to the right environment)
- [ ] Change list in the deployment pipeline reviewed line by line
- [ ] Release communication drafted (who needs to know and when)

## After Production

- [ ] Refresh succeeded in Production
- [ ] Spot-check three known numbers against the source
- [ ] Usage and errors reviewed after 24 hours
- [ ] Documentation and change log updated
