# Row-level security standards

## Goal

Let a regional leader see their facilities, a facility administrator see one facility, and finance see everything, using **one model and one report**, with access that is maintained as data, not as code.

## Standards

1. **Dynamic RLS with a security table.** Do not hard-code one role per facility. Use a small mapping table (user or group to facility or region) and a single role that filters on it.
2. **Group-based membership.** Map Entra ID groups to roles in the service, and manage people in the groups.
3. **Filter dimensions, not facts.** Apply the filter to the `Facility` dimension and let relationships propagate to the facts.
4. **Test before release.** Use "View as" for at least two personas (for example, a single-facility user and a regional user) and confirm totals change as expected.
5. **Document the roles.** Each certified model lists its roles, who owns the mapping, and how to request access.

## Reference pattern

Security table (one row per user or group and the facility they may see):

| user_principal_name | facility_id |
|---|---|
| region.leader@example.com | FAC-01 |
| region.leader@example.com | FAC-02 |
| facility.admin@example.com | FAC-03 |

In the model these columns are renamed to `SecurityMap[User Principal Name]` and `SecurityMap[Facility ID]`, following the [naming standards](modeling-and-naming-standards.md). Role filter on the `Facility` table:

```dax
[Facility ID] IN
    CALCULATETABLE (
        VALUES ( SecurityMap[Facility ID] ),
        SecurityMap[User Principal Name] = USERPRINCIPALNAME ()
    )
```

Use a separate, tightly controlled role or group for users who need every facility, instead of leaving the mapping table open.

## Things that catch people out

- RLS restricts users with **Viewer** access. Admins, Members, and Contributors of a workspace can see all data, so keep those roles small and deliver content to consumers through an app or Viewer access.
- Users with Build permission on a model can query it, and RLS still applies to them. Confirm that is the behavior you want before granting Build.
- RLS in the semantic model does not protect data in the lakehouse itself. Use workspace and item permissions in the data layer.
- Test with DirectQuery and Direct Lake sources separately, because security behavior can differ by storage mode. Check the current Microsoft documentation for your configuration.
- Object-level security (hiding columns or tables) is separate from RLS. Use it for fields such as identifiers that only some roles may see.

## Healthcare note

For regulated data, combine RLS with sensitivity labels, minimal necessary data in the model, no direct identifiers in report-facing tables, audit log review, and a documented access-request and access-review process (for example, quarterly). Review the design with your privacy and compliance teams.
