# Customize database configuration in Timescale Forge
Timescale Forge allows you to customize many TimescaleDB and PostgreSQL
configuration options for each Service individually. Most configuration values
for a Service are initially set in accordance with best practices given the
compute and storage settings of the Service. Any time you increase or decrease
the compute for a Service the most essential values are set to reflect the size
of the new Service.

There are times, however, when your specific workload may require tuning some
of the many available TimescaleDB and PostgreSQL parameters. By providing the
ability to tune various runtime settings, Timescale Forge provides the balance
and flexibility you need when running your workloads in our hosted environment.

<highlight type="warning">
Modifications of most parameters can be applied without restarting
the Timescale Forge Service. However, as when modifying the compute resources
of a running Service, some settings will require that a restart be performed,
resulting in some brief downtime (usually about 30 seconds).
</highlight>

### View service operation details
To modify configuration parameters, first select the Service that you want to
modify. This displays the `service details`, with these tabs across the top:
Overview, Operations, Metrics, Logs, and Settings. Select `Settings`.

<img class="main-content__illustration" src="https://assets.iobeam.com/images/docs/forge_images/timescale-forge-service-settings-basic.png" alt="View Timescale Forge service operational information"/>

### Modify basic parameters
Under the Settings tab, you can modify a limited set of the parameters that
are most often modified in a TimescaleDB or PostgreSQL instance. To modify a
configured value, click the value that you would like to change. This reveals
an editable field to apply your change. Clicking anywhere outside of that field
will save the value to be applied.

<img class="main-content__illustration" src="https://assets.iobeam.com/images/docs/forge_images/timescale-forge-service-settings-modify.png" alt="View Timescale Forge service settings modification"/>

### Apply configuration changes
When you have modified the configuration parameters that you would
like to change, click `Apply Changes`. For some changes, such as
`timescaledb.max_background_workers` (pictured below), the Service needs to be
restarted. In this case, the button reads `Restart and apply changes`.

<img class="main-content__illustration" src="https://assets.iobeam.com/images/docs/forge_images/timescale-forge-service-settings-apply.png" alt="View Timescale Forge service apply settings parameter changes"/>

Regardless of whether the Service needs to be restarted or not, a confirmation
dialog is displayed which lists the parameters that are being modified. Click
`Confirm` to apply the changes (and restart if necessary).

<img class="main-content__illustration" src="https://assets.iobeam.com/images/docs/forge_images/timescale-forge-service-settings-confirm.png" alt="View Timescale Forge service configuration changes confirmation dialog"/>

## Configuring advanced parameters
It is also possible to configure a wide variety of Service database parameters
by toggling `Show advanced parameters` in the upper-right corner of the
`Settings` tab.

<img class="main-content__illustration" src="https://assets.iobeam.com/images/docs/forge_images/timescale-forge-service-settings-advanced.png" alt="View Timescale Forge service configuration changes confirmation dialog"/>

Once toggled, a scrollable (and searchable) list of configurable parameters is
displayed.

<img class="main-content__illustration" src="https://assets.iobeam.com/images/docs/forge_images/timescale-forge-service-settings-advanced-search.png" alt="View Timescale Forge service configuration changes confirmation dialog"/>

As with the basic database configuration parameters, any changes are highlighted
and the `Apply changes` (or `Restart and apply changes`) button is available to
click, prompting you to confirm any changes before the Service is modified.

## PostgreSQL extensions
You can use PostgreSQL extensions with Timescale Forge. If you run
`pg_available_extensions` at the command prompt, the returned list of extensions
is inaccurate. To see the allowed extensions, use this command instead:
```sql
SELECT pae.* FROM current_setting('extwlist.extensions') AS cs(e) CROSS JOIN regexp_split_to_table(e, ',') AS ext(allowed) JOIN pg_available_extensions AS pae ON (allowed=name) ORDER BY 1;
```
These are the currently supported extensions:

|Extension|Description|
|---|---|
|bloom|Bloom access method - signature file based index|
|btree_gin|Support for indexing common datatypes in GIN|
|btree_gist|Support for indexing common datatypes in GiST|
|citext|Data type for case-insensitive character strings|
|cube|Data type for multidimensional cubes|
|dict_int|Text search dictionary template for integers|
|dict_xsyn|Text search dictionary template for extended synonym processing|
|fuzzystrmatch|Determine similarities and distance between strings|
|hstore|Data type for storing sets of (key, value) pairs|
|intarray|Functions, operators, and index support for 1-D arrays of integers|
|isn|Data types for international product numbering standards|
|lo|Large Object maintenance|
|ltree|Data type for hierarchical tree-like structures|
|pg_stat_statements|Track execution statistics of all SQL statements executed|
|pg_trgm|Text similarity measurement and index searching based on trigrams|
|pgcrypto|Cryptographic functions|
|pgrouting|pgRouting Extension|
|postgis|PostGIS geometry and geography spatial types and functions|
|postgis_raster|PostGIS raster types and functions|
|postgis_sfcgal|PostGIS SFCGAL functions|
|postgis_tiger_geocoder|PostGIS tiger geocoder and reverse geocoder|
|postgis_topology|PostGIS topology spatial types and functions|
|promscale|Promscale support functions|
|seg|data type for representing line segments or floating-point intervals|
|tablefunc|Functions that manipulate whole tables, including crosstab|
|tcn|Triggered change notifications|
|timescaledb_toolkit|TimescaleDB Toolkit|
|timescaledb|Enables scalable inserts and complex queries for time-series data|
|tsm_system_rows|TABLESAMPLE method which accepts number of rows as a limit|
|tsm_system_time|TABLESAMPLE method which accepts time in milliseconds as a limit|
|unaccent|Text search dictionary that removes accents|
|uuid-ossp|Generate universally unique identifiers (UUIDs)|
