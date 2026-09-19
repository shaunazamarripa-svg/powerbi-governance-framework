# Deployment pipeline workflow

## Stages

| Stage | Workspace | Who deploys | Gate to leave the stage |
|---|---|---|---|
| Develop | `ws_<domain>_dev` | Developer | Peer review done, Best Practice Analyzer scan clean or exceptions noted |
| Test | `ws_<domain>_test` | Platform team or model owner | UAT sign-off, data validation against source totals |
| Production | `ws_<domain>_prod` | Platform team | Release checklist complete, rollback noted |

## Rules

1. **Work only in Dev.** Nobody edits Test or Production content directly.
2. **Use deployment rules** to swap environment-specific settings (data source, lakehouse, parameters) so the same artifact can move unchanged between stages.
3. **Compare before you deploy.** Review the pipeline's change list, and read every changed item name.
4. **Validate after deploy.** Refresh the semantic model in the target stage and check row counts and a few known totals before announcing the release.
5. **Version the source.** Keep report and model definitions in Git (Fabric Git integration or PBIP project files) so every change has an author, a date, and a diff.
6. **Small, frequent releases** beat large, rare ones.

## Branching (when using Git integration)

| Branch | Purpose |
|---|---|
| `main` | Mirrors what is in Production |
| `feature/<short-name>` | One change, one branch, one pull request |

Pull requests need at least one reviewer who did not author the change.

## Peer review checklist

- [ ] Measures follow naming and formatting standards
- [ ] No new duplicate definitions of an existing certified metric
- [ ] Relationships are single-direction unless justified in the description
- [ ] RLS roles tested with "View as" for at least two different users or groups
- [ ] Refresh schedule and failure notification set
- [ ] Best Practice Analyzer scan run (see [BestPracticeRules.json](../tabular-editor/BestPracticeRules.json))

## Rollback

Because production is only ever deployed to, rollback is a redeploy of the last known good version from the previous stage or from Git. Record the version you would roll back to in the [release checklist](../templates/release-checklist.md) before you deploy.

## Notes

Pipeline capabilities and supported item types evolve. Check current Microsoft documentation for which items your tenant can deploy automatically, and script or document the manual steps for the rest.
