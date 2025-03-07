# Percentile approximation
Examining time-series data through percentiles is useful for understanding the
distribution of your time-series data. Specifically, they can help eliminate the
inherent impact that outliers have on calculations such as average. For instance
the 50% percentile (median) of the data can be a more useful measure than
average when there are outliers that would dramatically impact the average, but
have a much smaller impact on the median. The median or 50th percentile means
that in an ordered list of your data half of the data will have a greater value
and half a smaller value. Likewise, the 10th percentile would mean that 10% fall
below and 90% above the value returned.

Often the 95th or 99th percentile can be very useful in identifying normalized
trends in networking and monitoring applications. For instance, when a user reports
that your website is taking 30 second to load, it's helpful to quickly identify
that 99% of requests occur in 200ms or less, which means that this specific
report is an outlier and likely caused by extraordinary conditions.

By using percentiles, outliers have less of an impact on the calculations because
their magnitude doesn't affect their percentile, only their order in the set.
Therefore, the skew that is introduced to calculations like `AVG()`  by infrequent
very large or very small values is reduced or eliminated.

We provide percentile approximation functions because exact percentiles are not
parallelizable, cannot be used with continuous aggregates and would be very
inefficient when used with multi-node TimescaleDB. Our percentile approximation
algorithm provide good estimates of percentiles while integrating much more fully
with all these other TimescaleDB features.

## Using percentile approximation in TimescaleDB

<highlight type="tip">
In order to use functions in the TimescaleDB Toolkit, ensure that
the [extension is installed](/timescaledb/latest/how-to-guides/install-timescaledb-toolkit/) and available within your database.
</highlight>

Percentiles in TimescaleDB are calculated in two steps. First, we
must create a percentile estimator which can be created using either
[`percentile_agg()`][percentile_agg],
or one of the [advanced aggregation methods][advanced_agg_methods] `uddsketch()` or `tdigest()`. Estimators can be combined or re-aggregated using the [rollupfunction][rollup].

Once the estimator is created, the desired values can be obtained by using the aggregate result as
input to the following functions: [](#percentile-accessors)

 * [`approx_percentile()`](/hyperfunctions/percentile-approximation/approx_percentile)
 * [`approx_percentile_rank()`](/hyperfunctions/percentile-approximation/approx_percentile_rank)
 * [`mean()`](/hyperfunctions/percentile-approximation/mean)
 * [`error()`](/hyperfunctions/percentile-approximation/error)
 * [`num_vals()`](/hyperfunctions/percentile-approximation/num_vals)

Additionally, the output of the aggregation methods can be stored as part of a
continuous aggregate for re-aggregation using the above value functions.

[percentile_agg]: /hyperfunctions/percentile-approximation/percentile_agg/
[advanced_agg_methods]: /hyperfunctions/percentile-approximation/percentile-aggregation-methods/
[rollup]: /hyperfunctions/percentile-approximation/rollup-percentile
