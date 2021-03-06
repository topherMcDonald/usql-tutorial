## Basic Statistics with MAX, MIN, AVG, STDEV, & SUM

These do what you expect them to do

```
@output =
    SELECT
        MAX(Duration) AS DurationMax,
        MIN(Duration) AS DurationMin,
        AVG(Duration) AS DurationAvg,
        SUM(Duration) AS DurationSum,
        VAR(Duration) AS DurationVariance,
        STDEV(Duration) AS DurationStDev,
        VARP(Duration) AS DurationVarianceP,
        STDEVP(Duration) AS DurationStDevP
    FROM @searchlog
    GROUP BY Region;
    ```

### Notes for Statisticians

**VAR** & **STDEV** are the **sample version** with Bessel's correction
**VARP** & **STDEVP** are the better-known **population version**.
