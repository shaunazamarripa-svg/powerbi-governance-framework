# Workspace and environment standards

## Content tiers

| Tier | Purpose | Where it lives | Endorsement | Support |
|---|---|---|---|---|
| Personal | Exploration by one person | My workspace | None | None |
| Team | Shared by a department | `ws_<team>_<purpose>` | Optional: Promoted | Owning team |
| Enterprise | Cross-department, decision-grade | `ws_<domain>_<env>` with pipelines | Certified | Platform and data owner |

Personal and team content is welcome. Anything used for enterprise decisions moves up a tier.

## Workspace layout

| Pattern | Contains | Notes |
|---|---|---|
| `ws_<domain>_dev`, `_test`, `_prod` | Data items (lakehouses, notebooks, pipelines) and semantic models | Deployed via pipeline |
| `ws_<domain>_reports_dev`, `_test`, `_prod` | Reports and apps built on the models above | Separating models from reports lets many reports share one certified model and limits who can change definitions |

Use lowercase and no spaces, and name by domain (for example `snf`, `finance`, `hr`) rather than by team, because teams reorganize and domains do not.

## Roles and access

- Grant access to **Entra ID security groups**, never to individuals. One group per workspace per role, for example `sg-pbi-snf-prod-viewers`.
- Suggested mapping:

| Workspace | Admin | Member / Contributor | Viewer |
|---|---|---|---|
| Dev | Platform team | Developers | Nobody else |
| Test | Platform team | Platform team, model owners | UAT testers |
| Prod | Platform team (break-glass) | Nobody (deployment service only) | Consumers via app or group |

- Production consumers should get content through an **app** or a Viewer group so that row-level security applies to them. Workspace roles above Viewer bypass RLS, so keep those roles tight.
- Keep at least two Admins per workspace so ownership survives staff changes.

## Sensitivity and endorsement

- Apply **sensitivity labels** to semantic models, reports, and lakehouses that contain regulated data. Labels follow exports and downstream content.
- **Promoted**: the owner believes it is ready for wider use. **Certified**: it passed the [certification checklist](certification-checklist.md) and was approved by a designated certifier.
- Restrict who can certify through the tenant setting for certification.

## Lifecycle rules

- Every workspace has a named owner and a documented purpose.
- Review each enterprise workspace quarterly: usage, owners, stale items.
- Retire content that has had no views for a defined period (for example 180 days) after notifying the owner. Archive first, then delete.
