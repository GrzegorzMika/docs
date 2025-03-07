## rollup()

```SQL
rollup(
    sketch uddsketch
) RETURNS UddSketch
```
```SQL
rollup(
    digest tdigest
) RETURNS tdigest
```

This combines multiple outputs from the
[`percentile_agg()` function][percentile_agg] (or either
[`uddsketch()` or `tdigest()`][advanced_agg_methods]). This is especially
useful for re-aggregation in a continuous aggregate. For example, bucketing by a larger [`time_bucket()`][time_bucket], or re-grouping on other dimensions
included in an aggregation.

### Required arguments

|Name|Type|Description|
|---|---|---|
|`sketch` / `digest` |`UddSketch` or `tdigest` |The already constructed data structure from a previous `percentile_agg`, `uddsketch`, or `tdigest` call|

### Returns

|Column|Type|Description|
|---|---|---|
|`rollup`|`UddSketch` / `tdigest`|A UddSketch or tdigest object which may be passed to further APIs|

Because the `percentile_agg()`](/hyperfunctions/percentile-approximation/aggregation-methods/percentile_agg/) function uses the [UddSketch algorithm](/hyperfunctions/percentile-approximation/aggregation-methods/uddsketch/), `rollup` returns the UddSketch data structure for use in further calls.

When using the `percentile_agg` or `UddSketch` aggregates, the `rollup` function will not introduce additional error (compared to calculating the estimator directly), however, using `rollup` with `tdigest` may introduce additional error compared to calculating the estimator directly on the underlying data.

### Sample usage
Here, we re-aggregate an hourly continuous aggregate into daily buckets, the usage with `uddsketch` & `tdigest` is analogous:
```SQL
CREATE MATERIALIZED VIEW foo_hourly
WITH (timescaledb.continuous)
AS SELECT
    time_bucket('1 h'::interval, ts) as bucket,
    percentile_agg(value) as pct_agg
FROM foo
GROUP BY 1;

SELECT
    time_bucket('1 day'::interval, bucket) as bucket,
    approx_percentile(0.95, rollup(pct_agg)) as p95,
    approx_percentile(0.99, rollup(pct_agg)) as p99
FROM foo_hourly
GROUP BY 1;
```

[percentile_agg]: /hyperfunctions/percentile-approximation/percentile_agg/
[advanced_agg_methods]: /hyperfunctions/percentile-approximation/percentile-aggregation-methods/
[time_bucket]: /hyperfunctions/time_bucket/
