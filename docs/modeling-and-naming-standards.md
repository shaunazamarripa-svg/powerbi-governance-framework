# Modeling and naming standards

## Model shape

- **Star schema.** Facts in the middle, dimensions around them. Avoid snowflaking and many-to-many relationships unless there is no alternative, and document the reason when there is not.
- **One grain per fact table**, stated in the table description (for example, "One row per facility, payer, and day").
- **Single-direction relationships**, one-to-many from dimension to fact. Bidirectional filtering needs a written justification.
- **A dedicated date dimension**, marked as a date table. Turn off auto date/time.
- **Hide technical columns** (keys, sort columns, lineage). Report authors should see only what they should use.
- **Do the work upstream.** Shape data in the lakehouse (see [fabric-lakehouse-blueprint](https://github.com/shaunazamarripa-svg/fabric-lakehouse-blueprint)) rather than in Power Query, so it is reusable and testable.

## Naming

| Object | Convention | Example |
|---|---|---|
| Tables | Business nouns, singular for dimensions, no prefixes in the model | `Facility`, `Payer`, `Census` |
| Columns | Title Case with spaces for anything visible | `Admit Date`, `Payer Category` |
| Measures | Title Case, self-explanatory, no abbreviations | `Average Daily Census`, `Days in AR` |
| Hidden technical columns | Keep source names | `facility_id` |
| Parameters | `p_` prefix | `p_environment` |
| Display folders | One per business subject | `Census`, `Revenue`, `AR` |

## Measures

- Every visible measure has a **format string** and a **description** that states the definition in business terms.
- Put base measures in a dedicated measures table or folder; build others from them.
- Prefer measures over calculated columns for aggregations. Use calculated columns only when a value must be sliced on.
- Use variables (`VAR`) for readability and to avoid repeated evaluation.
- Use `DIVIDE()` instead of `/` to handle zero denominators.

Example of a well-documented measure (generic, using the synthetic schema):

```dax
-- Description: Average number of residents in-house per day over the selected period.
Average Daily Census =
DIVIDE ( SUM ( Census[Census Count] ), DISTINCTCOUNT ( 'Date'[Date] ) )
```

## Refresh and performance

- Set a refresh schedule and a failure notification recipient on every enterprise model.
- Remove unused columns and tables. Size and refresh time are governed like any other cost.
- Prefer numeric surrogate or integer keys for relationships.
- Run the Best Practice Analyzer rules in [BestPracticeRules.json](../tabular-editor/BestPracticeRules.json) before every deployment.

## Documentation

Each certified model has a short README in its workspace or repo: purpose, owner, source lineage, refresh schedule, metric definitions, RLS roles, and known limitations.
