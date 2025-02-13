The SQL statistics feature of CLS can analyze the collected logs and display the analysis results using different types of charts, such as line chart, bar chart, and pie chart. You can choose any type as needed.

## Analysis Syntax

>!To use the log analysis feature, you must configure the key-value index of statistical fields and enable statistics.


An analysis statement is composed of search criterion and SQL statement, which are separated by a vertical bar (|).
```
[Search criterion] | [SQL statement]
```
**Examples**
```
Calculate the page views (PV) without a search criterion.
* | select count(*) as pv
```
```
Calculate the PV with status code 404 as the search criterion.
status:404 | select count(*) as pv
```
- Currently, SQL statements support the `SELECT`, `AS`, `GROUP BY`, `ORDER BY`, `LIMIT`, and `WHERE` syntax. For more information, see [SQL syntax](#sql1).
- You don’t need to set the `FROM` clause for SQL statements. Data from the current log topic is analyzed by default.
- You don’t need to add a semicolon at the end of an SQL statement to indicate the end.
- SQL statements are case-insensitive.



## Directions

1. Log in to the [CLS console](https://console.cloud.tencent.com/cls).
2. Select **Search and Analysis** on the left sidebar to go to the search and analysis page.
3. Select the logset and log topic you want to search.
   Set the time range. You can choose last 1 hour, last 4 hours, last 1 day, or last 3 days, or custom a time range. 
4. Enter an analysis statement and click **Search and Analysis** to obtain the analysis result.
   For more information about analysis statements, see **[SQL syntax](#sql1)** and **[SQL functions](#sql2)**. 





## Supported SQL Syntax and Functions

CLS supports the following SQL syntax and functions. You can click them to view details.

<span id="sql1"></span>

#### SQL syntax

| Syntax | Description |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| [SELECT](https://intl.cloud.tencent.com/document/product/614/38735) | Selects data from tables. |
| [AS](https://intl.cloud.tencent.com/document/product/614/38731)   | Specifies an alias for a column (KEY). |
| [GROUP BY](https://intl.cloud.tencent.com/document/product/614/38732) | Combines aggregate functions to group results based on one column (KEY) or more. |
| [ORDER BY](https://intl.cloud.tencent.com/document/product/614/38734) | Sorts results according to the specified `KEY`. |
| [LIMIT](https://intl.cloud.tencent.com/document/product/614/38733) | Limits the amount of data returned by the `SELECT` statement. |
| [WHERE](https://intl.cloud.tencent.com/document/product/614/38736) | Extracts the logs that meet the specified criteria. |


<span id="sql2"></span>

####  SQL functions

- [Aggregate Function](https://intl.cloud.tencent.com/document/product/614/38728)
  Support `avg`, `count`, `max`, `min`, and `sum` functions.
- [Mathematical Function](https://intl.cloud.tencent.com/document/product/614/38727)
  Support `abs`, `sqrt`, `power`, `round`, `floor`, `log`, and `log10` functions.
- [Datetime Function](https://intl.cloud.tencent.com/document/product/614/36746)
  Support `cast` and `histogram` functions.
- [Operator](https://intl.cloud.tencent.com/document/product/614/38729)
  Support mathematics operators (+, -, *, /, %), comparison operators (=, number comparison, in, like, etc.), and logic operators (and, or, nor).

## SQL Limits

| Metric | Limit | Remarks |
| ---------------------------------------- | ------------------------------------------------------------ | --------------------------- |
| Maximum length of a single statistical field (Value) | The maximum length of a single statistical field (Value) is 32 KB. Any statement exceeding this limit will be truncated. | - |
| Number of non-cluster analysis results (excluding `GROUP BY` clauses) | Each analysis returns up to 1,000 results. | Default: 100; Maximum: 1,000 |
| Number of cluster analysis results (including `GROUP BY` clauses) | Each analysis returns up to 100 results. | Default: 100; Maximum: 2,000 |
| `ORDER BY` clause limit | The `ORDER BY` operation is not supported for fields that have been calculated using one of the functions `round`, `sqrt`, `abs`, `power`, and `floor`, or the four arithmetic operations of addition, subtraction, multiplication, and division. | - |
