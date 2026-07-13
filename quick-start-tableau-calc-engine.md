## Quick Start

### Prerequisites

- [Claude Desktop](https://claude.ai/download) with agent mode enabled
- [Tableau MCP](https://github.com/tableau/tableau-mcp) installed and authenticated
- Access to a Tableau Cloud or Tableau Server site

### Install

1. Clone or download this repository.
2. Copy the skill folder into your Claude skills directory.
3. Restart Claude Desktop.
4. Confirm Tableau MCP is connected (you should see your site's content when you ask Claude about it).

### Example Prompts

**Write a new calculation:**

```
Write a YoY growth calc for my Sales measure, using fiscal year starting in July
```

```
I need an LOD expression that finds each customer's first order date, then calculates days since first purchase
```

**Debug an existing one:**

```
This calc returns NULL when the filter is applied — what's wrong?
IF [Region] = "West" THEN {FIXED [Customer] : SUM([Sales])} END
```

**Translate from SQL:**

```
Translate this to Tableau:
SELECT region, SUM(sales) / COUNT(DISTINCT customer_id) AS revenue_per_customer
FROM orders WHERE order_date >= '2024-01-01' GROUP BY region
```

**Optimize:**

```
This table calc is making my dashboard slow — can you rewrite it as an LOD?
RUNNING_SUM(SUM([Quantity])) / TOTAL(SUM([Quantity]))
```

### What You Get

A paste-ready calculated field with: the formula, inline comments, type classification (row-level, aggregate, LOD, or table calc), placement in Tableau's order of operations, version requirements, and performance notes. Includes where to place it (rows, columns, color, filter) and any dependencies on other fields.
