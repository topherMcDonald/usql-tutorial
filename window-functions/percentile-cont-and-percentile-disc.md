# PERCENTILE\_CONT & PERCENTILE\_DISC

These two functions calculates a percentile based on a continuous or discrete distribution of the column values.

* **PERCENTILE_CONT** calculates a percentile based on a **continuous distribution**. It will interpolate values. 

* **PERCENTILE_DISC** calculates the percentile based on a **discrete distribution**. It  will always return one of the input values and will not interpolate a value.

Syntax

```
[PERCENTILE_CONT | PERCENTILE_DISC] ( numeric_literal )
    WITHIN GROUP ( ORDER BY <identifier> [ASC | DESC] )
    OVER ( [PARTITION BY <identifier,>…[n] ] ) AS <alias>
```

* `numeric_literal` - The percentile to compute. range = [0.0, 1.0].

* `WITHIN GROUP ( ORDER BY <identifier> [ASC | DESC])` - Within each partition, compute the percentile on `identifier`. The default sort order is ascending.

* `OVER ( [PARTITION BY <identifierm,...> [n] ] )` - Defines the partitions. 

Note: Any nulls in the data set are ignored.

### PERCENTILE_CONT vs PERCENTILE_DISC

You can see how **PERCENTILE_CONT** and **PERCENTILE_DISC** differ in the example below which tries to find the median \(percentile=0.50\) value for Latency within each Vertical

```
@result =
    SELECT
        Vertical,
        Query,
        PERCENTILE_CONT(0.5) 
            WITHIN GROUP (ORDER BY Latency)
            OVER ( PARTITION BY Vertical ) AS PercentileCont50,
        PERCENTILE_DISC(0.5)
            WITHIN GROUP (ORDER BY Latency)
            OVER ( PARTITION BY Vertical ) AS PercentileDisc50
    FROM @querylog;
```

The results:

| **Query** | **Latency** | **Vertical** | **PercentileCont50** | **PercentilDisc50** |
| :--- | :--- | :--- | :--- | :--- |
| Banana | 300 | Image | 300 | 300 |
| Cherry | 300 | Image | 300 | 300 |
| Durian | 500 | Image | 300 | 300 |
| Apple | 100 | Web | 250 | 200 |
| Fig | 200 | Web | 250 | 200 |
| Papaya | 200 | Web | 250 | 200 |
| Fig | 300 | Web | 250 | 200 |
| Cherry | 400 | Web | 250 | 200 |
| Durian | 500 | Web | 250 | 200 |

Look at the median for the `Web` vertical.

* **PERCENTILE_CONT** gives the median as 250 even though no query in the web vertical had a latency of 250.
* **PERCENTILE_DISC** gives median for Web as 200, which is an actual value found in the input rows.

