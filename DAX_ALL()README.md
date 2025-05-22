# Understanding the DAX ALL() Function

## Purpose of ALL()

The `ALL()` function in Data Analysis Expressions (DAX) is a powerful tool used to **remove filters** from a column or an entire table within a calculation. This is crucial for scenarios where you need to calculate values independently of the current filter context applied by visuals, slicers, or other filters in your Power BI or tabular model.

### Key Use Cases:

* **Showing totals or benchmarks:** Calculate grand totals that remain constant regardless of specific filter selections.
* **Calculating % of total:** Determine a measure's contribution relative to a grand total.
* **Ranking:** Rank items based on a measure, ignoring filters that might otherwise restrict the ranking to a subset.
* **Creating cumulative totals:** Sum values over a continuous range, typically dates, independent of the current individual date filter.
* **Implementing comparisons:** Facilitate comparisons like "Sales this year vs. total sales last year."

## Common Use Cases & Examples

### 1. Percentage of Total

Calculates what percentage of the grand total a specific filtered value represents.

```Code snippet

Sales % of Total = 
DIVIDE(
    [Total Sales],
    CALCULATE([Total Sales], ALL(Sales))
)
```
Explanation: ALL(Sales) removes all existing filters from the Sales table, allowing CALCULATE([Total Sales], ALL(Sales)) to return the grand total sales, irrespective of any product, category, or region filters applied.

### 2. Ignore Filter on Specific Column
Removes filters from a particular column while keeping other filters active.

```Code snippet

Total Sales All Years = 
CALCULATE([Total Sales], ALL(Date[Year]))
```
Explanation: ALL(Date[Year]) specifically removes any filter applied to the Year column of the Date table, enabling the calculation of total sales across all years, while other filters (e.g., specific regions or products) might still apply.

### 3. Ranking Within a Group
Ranks items (e.g., products) based on a measure, ignoring current filters on the ranking dimension.

```Code snippet

Rank by Sales = 
RANKX(
    ALL(Product[ProductName]),
    [Total Sales]
)
```
Explanation: ALL(Product[ProductName]) ensures that RANKX considers all product names when assigning ranks, even if a slicer or filter is applied to a specific product category. This allows for a global ranking across all products.

### 4. Cumulative Total
Calculates a running total over a specific period, typically dates.

```Code snippet

Cumulative Sales = 
CALCULATE(
    [Total Sales],
    FILTER(
        ALL(Date),
        Date[Date] <= MAX(Date[Date])
    )
)
```
Explanation: ALL(Date) removes the implicit filter context from the Date table, allowing the FILTER function to iterate over all dates. Date[Date] <= MAX(Date[Date]) then accumulates sales up to the latest date visible in the current filter context (e.g., the current row in a table visual).

### Difference: ALL() vs REMOVEFILTERS() vs ALLEXCEPT()
These functions are often used for similar purposes but have distinct behaviors:

###  Function	What it Does
ALL()	Removes all filters from a specified table or all filters from a specified column.
REMOVEFILTERS()	Semantically similar to ALL() but often preferred for clarity in newer DAX.
ALLEXCEPT()	Removes all filters except those on specified columns within a table.
