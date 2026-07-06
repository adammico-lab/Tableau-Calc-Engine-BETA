# Tableau Calc Engine

**Tableau Calculation Authoring & Optimization | v1.1**

---

## What It Does

Tableau Calc Engine writes, debugs, translates, and optimizes Tableau calculated fields. Given a description of what you need, it produces a complete, commented, paste-ready formula — correctly typed (row-level, aggregate, LOD, or table calc), positioned in Tableau's order of operations, and named within 20 characters. When something's broken, it diagnoses why. When you have SQL and need Tableau, it translates. When it's slow, it rewrites for performance.

It covers the full calculation surface: basic expressions, LOD (FIXED/INCLUDE/EXCLUDE), table calculations (addressing, partitioning, nesting), sets and set actions, dynamic zone visibility, spatial/mapping functions (MAKEPOINT, MAKELINE, DISTANCE, BUFFER), string functions (REGEXP, SPLIT, FIND), parameters, and parameter actions.

---

## What's New in v1.1

**Sets & Set Actions** — Full coverage of computed sets, combined sets, set actions (2018.3+), set-based calculations, and the critical order-of-operations interaction (sets live at step 5: FIXED ignores them, INCLUDE/EXCLUDE respects them).

**String Functions Reference** — Core functions (REGEXP_EXTRACT, REGEXP_REPLACE, SPLIT, MID, FIND, TRIM) plus a SQL → Tableau equivalence table for Translate mode.

**Last N Complete Periods recipe** — The #1 most-asked date calc: filter to last N full months, excluding the partial current month.

**Relative Date Window recipe** — Compare last 30 days vs. prior 30 days (configurable window, non-calendar-aligned).

**Version Compatibility Table** — Feature-to-minimum-version mapping at the top of the skill. Inline `// Requires Tableau 2022.3+` notes on version-gated recommendations.

**Formula accuracy fixes** — Three incorrect formulas from v1.0 corrected (FIRST() usage, invalid RANK_DENSE in LOD, Prior Period CASE structure).

**Additional edge cases** — Data model (relationships vs. single table) behavior, string comparison case sensitivity, version-gated feature handling.

---

## What It Does NOT Do

**It does not design dashboards.** Calc Engine writes the formulas — it doesn't tell you which charts to build, where to place them, or how to structure a layout. That's Tableau Dashboard Blueprint.

**It does not evaluate finished dashboards.** It doesn't look at your completed work and score it. That's VizCritique Pro.

**It does not generate test data.** If you need synthetic data to test your calcs against, that's Hyper Synthetic Forge.

**It does not replace Tableau Desktop.** You still paste formulas into Tableau, configure table calc settings (Compute Using), and set up parameters manually. The skill gives you production-ready logic — you implement it.

**It does not write Tableau Prep calculations.** Prep uses a different formula syntax and context. This skill targets Desktop/Server calculated fields.

**It does not modify your datasource schema.** When it recommends "push to datasource," it tells you what to materialize and why — you or your data engineer implements that change.

---

## Four Operating Modes

| Mode | Input | Output |
|------|-------|--------|
| **Write** | "I need a calc that does X" | Complete formula with name, type, comments, dependencies, and placement guidance |
| **Debug** | "This calc returns NULL / wrong values / errors" | Diagnosis, root cause, corrected formula, and prevention advice |
| **Translate** | "Convert this SQL to Tableau" | SQL breakdown, Tableau strategy per SQL concept, complete formula(s), and behavioral caveats |
| **Optimize** | "This calc is slow on 2M rows" | Performance diagnosis, rewritten formula, alternative architectures, and rules applied |

---

## What It Covers

| Domain | Includes |
|--------|----------|
| **Basic Calcs** | String, date, logic, arithmetic, type conversion, NULL handling |
| **Aggregate Expressions** | SUM, AVG, COUNTD, MIN, MAX, custom aggregations |
| **LOD Expressions** | FIXED, INCLUDE, EXCLUDE — when to use each, nesting, filter interaction |
| **Table Calculations** | Addressing, partitioning, LOOKUP, RUNNING_SUM, RANK, WINDOW, INDEX, nested table calcs |
| **Sets & Set Actions** | Computed sets, combined sets, set actions, IN/OUT calcs, set + order-of-operations interaction |
| **Dynamic Zone Visibility** | Boolean calcs for show/hide zones, parameter-action patterns, role-based visibility |
| **Spatial & Mapping** | MAKEPOINT, MAKELINE, DISTANCE, BUFFER, AREA, LENGTH, hex binning, spatial joins |
| **String Functions** | REGEXP_EXTRACT/REPLACE/MATCH, SPLIT, MID, FIND, TRIM, CONTAINS + SQL equivalences |
| **Parameters** | Dynamic measure/dimension swap, TOP N, reference lines, parameter actions |
| **Order of Operations** | Full pipeline understanding — where each calc type lives and why it matters |
| **Performance** | Optimization hierarchy, when to push to datasource, anti-patterns |

---

## Version Requirements

Not all features are available in all Tableau versions. The skill flags these automatically:

| Feature | Minimum Version |
|---------|----------------|
| LOD Expressions | 9.0 (2015) |
| Set Actions | 2018.3 |
| MAKEPOINT, MAKELINE | 2019.2 |
| Parameter Actions | 2019.2 |
| DISTANCE | 2020.1 |
| Dynamic Zone Visibility | 2022.3 |
| BUFFER | 2024.1 |

---

## Required & Companion Skills

| Skill | Relationship | Why |
|-------|-------------|-----|
| **Tableau MCP** (connector) | **Required for validation** | Calc Engine validates field names and tests logic against live datasources via `get-datasource-metadata` and `query-datasource`. Without it, the skill still writes calcs — just without live validation. |
| **Tableau Dashboard Blueprint** | **Recommended** | Blueprint identifies which calcs a dashboard needs. Calc Engine picks up those specs and produces production-ready formulas. Direct handoff when both are used in the same session. |
| **VizCritique Pro** | **Optional** | Evaluates the built result. If VizCritique flags a performance issue caused by calculations, Calc Engine can fix it. |
| **Hyper Synthetic Forge** | **Optional** | Generates test data to validate calc behavior before connecting production data. |

**Minimum viable setup:** Tableau Calc Engine installed. Works standalone — you describe what you need, it writes the formula.

**With Tableau MCP:** Validates field names against the live schema and tests logic against real data. If unavailable, the skill informs you and lists specific test cases to verify manually.

**Full suite:**
```
Hyper Synthetic Forge → Tableau Dashboard Blueprint → Tableau Calc Engine → Build in Tableau → VizCritique Pro
(generate data)         (design what to build)        (write the calcs)     (you build it)     (evaluate result)
```

---

## Recipe Library (Built-In Patterns)

The skill includes a library of production-tested calculation patterns that it draws from rather than deriving from scratch each time:

| Category | Patterns |
|----------|----------|
| **Time Intelligence** | YoY % change, prior period comparison (flexible by parameter), YTD running total with reset, fiscal year handling, last N complete periods, relative date window (last 30 vs. prior 30) |
| **Cohort Analysis** | Cohort assignment, periods-since-first, cohort size, retention rate |
| **Ranking & TOP N** | Dynamic TOP N with "Other" bucket, rank with ties, percentile rank |
| **% of Total** | Table calc (TOTAL), FIXED grand total, EXCLUDE parent %, all three methods with filter-response tradeoffs |
| **Moving Calculations** | Configurable moving average with NULL handling at edges |
| **Status & Alerts** | Traffic light KPI, variance direction indicators, threshold-based conditional formatting |
| **Customer Metrics** | Days since last event, lifetime value, order sequence number, distinct count with conditions (frequent buyers) |
| **52-Week Analysis** | 52-week high/low, % from trailing high |
| **Dynamic Swap** | Measure swap, dimension swap, parameter-driven chart configuration |
| **Spatial** | Distance calculations, delivery radius, drive-time proxy, hex binning for density |
| **Dynamic Zones** | Toggle panels, click-to-reveal, threshold-triggered alerts, role-based visibility |
| **Sets** | IN/OUT classification, % from set, set member count, set-driven KPI BANs |

---

## How to Trigger

Say any of:
- "Tableau Calc Engine"
- "Write a calc for..."
- "LOD expression"
- "Fix this table calc"
- "Why is this returning NULL?"
- "Convert this SQL to Tableau"
- "This calc is slow"
- "Dynamic zone visibility"
- "MAKEPOINT / DISTANCE / spatial calc"
- "Help me with addressing and partitioning"
- "Set action"
- "Order of operations"

---

## What It Needs From You

**For Write mode:**
- What the calculation should do (in plain English)
- Field names involved (or datasource name/LUID for auto-detection)
- Context: what's in the view? (Which dimensions are on shelves? This matters for table calcs and LOD grain.)

**For Debug mode:**
- The formula that's not working
- What it's returning vs. what you expect
- What's in the view (dimensions, filters)

**For Translate mode:**
- The SQL query or expression
- Target context: what does the Tableau view look like?

**For Optimize mode:**
- The slow formula
- Data volume (approximate row count)
- Where it's used (how many views, complexity of the dashboard)

If anything is ambiguous, the skill asks — it never guesses at business logic.

---

## Naming & Documentation Standards

Every calc produced follows these standards:

- **Name:** ≤20 characters, informative, uses consistent prefix convention
- **Comments:** Every non-trivial calc gets a header comment (purpose, dependencies, type, notes)
- **Type classification:** Explicitly labeled as row-level, aggregate, LOD, or table calc
- **Order of operations position:** Noted so you understand filter interaction behavior
- **Version note:** If the calc requires a specific Tableau version, it's flagged inline

---

## Performance Guidance

The skill ranks calculation approaches from fastest to slowest:

1. Datasource-materialized columns (pre-computed)
2. Row-level calculations
3. Simple aggregates
4. FIXED LOD
5. INCLUDE/EXCLUDE LOD
6. Basic table calculations
7. Nested LOD
8. Complex table calculations
9. String operations on high-cardinality fields
10. Nested table calcs with LOD inputs

When writing any calc, it considers where in this hierarchy the formula falls. If it lands in the bottom half and the user mentions large data, it proactively suggests alternatives or recommends pushing to the datasource.

---

## Use Cases

**Greenfield calculation authoring** — You know what you need but not how to express it in Tableau. Describe it in English, get a paste-ready formula.

**Debugging mysterious NULLs** — Your calc "worked yesterday" but now shows blanks everywhere. Calc Engine identifies whether it's addressing, filter timing, or scope.

**SQL-to-Tableau migration** — Your analyst built logic in SQL and you need the Tableau equivalent that respects the viz's grain and filter behavior. Includes string function equivalence mapping.

**Performance rescue** — A dashboard takes 45 seconds to load and the performance recorder points at calculations. Calc Engine identifies the expensive patterns and rewrites them.

**Dynamic interactivity** — You want zones that appear/disappear, measures that swap on click, sets that update on selection, TOP N that adjusts with a slider. Calc Engine writes the parameter + calc + set combinations to make it work.

**Spatial analysis** — You have lat/long data and need distance calculations, service area buffers, or hex-binned density maps. Version requirements noted.

**Learning the order of operations** — You keep getting burned by FIXED ignoring filters, sets not affecting LODs, or table calcs showing NULLs. Calc Engine explains the *why* and teaches the mental model, not just the fix.

---

## Design Principles

1. **Correct first, fast second.** A correct slow calc is better than a wrong fast one. Optimize after validation.
2. **Ask, don't assume.** Ambiguous business logic gets a clarifying question, not a guess.
3. **Simplest approach wins.** If SUM/AVG does the job, don't use LOD. If LOD does it, don't nest. Complexity must be justified.
4. **Comment everything non-obvious.** The next person reading this calc (including future-you) should understand it in 10 seconds.
5. **Name within 20 characters.** If you can't name it concisely, it might be doing too much — consider splitting.
6. **Respect the pipeline.** Every calc type has a position in Tableau's order of operations. Choose the type that lives at the right point for your use case.
7. **Push down when possible.** If a row-level calc runs on millions of rows and rarely changes, it belongs in the datasource — not in Tableau.
