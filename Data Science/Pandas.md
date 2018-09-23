# Indexing

## Selecting Columns with `df[[list]]`

## Label-Based Index with `df.loc[[row],[column]]`

## Boolean Indexing

You can pass boolean masks to regular `[]`, `.loc`, or `.iloc`.

Boolean indexers are useful because so many operations can produce an array of booleans.

- null checks (`.isnull`, `.notnull`)
- container checks (`.isin`)
- boolean aggregations (`.any`, `.all`)
- comparisions (`.gt`, `.lt`, etc.)

## Position Based index with .iloc

df.iloc[[row],[column]]

## Dropping Rows or Columns

df.drop([labels],axis=0)

# Alignment & Operations

## Alignment

- Working with multiple pandas objects
- Structuring your data to make analysis easier
- Using labels to their full potential



Alignment is a key part of many parts of pandas, including

- binary operations (`+, -, *, /, **, ==, |, &`) between pandas objects
- merges / joins / concats
- constructors (`pd.DataFrame`, `pd.Series`)
- reindexing



Pandas automatically deal with the alignment problem in situations where there are properly named label:

1. pd.read_csv(index_col=‘’)
2. df.set_index(‘<column name>’)

**In addition, Pandas align both the index and the column at the same time!!!**

## Handling Missing Data

Detecting Missing Data

1. `pd.isna(), df.isna()`
2. `pd.notna(), df.notana()`



Dropping missing Data

1. df.dropna(axis=<‘index’ or ‘columns’> how=<‘all’ or ‘any’>)



Filling missing Data

1. df.fillna(<value or method=‘ffill’>)

## Joining Pandas Objects

You have some options:

1. `pd.merge`: SQL-style joins when doing database style joins that are one-to-many, or many-to-many, or whenever you're joining on a column
2. `pd.concat`: array-style joins. use `concat` for one-to-one joins of two or more Series/DataFrames, where your joining on the index



## Reductions

`DataFrame` has many methods that *reduce* a DataFrame to a Series by aggregating over a dimension. Likewise, `Series` has many methods that collapse down to a scalar. Some examples are`.count`, `.mean`, `.std`,`.var`, `.max`, `.any`, `.all`,`.sum`,`.median`.

By default, the index (0) axis is reduced for `DataFrames` but can be specificly declared with `axis='columns'`

the `axis` argument specifies the axis you want to *remove*.



# Groupby

## Namespaces

- `.str`: defined on `Series` and `Index`es containing strings (object dtype)
- `.dt`: defined on `Series` with `datetime` or `timedelta` dtype
- `.cat`: defined on `Series` and `Indexes` with `category` dtype
- `.plot`: defined on `Series` and `DataFrames`

```python
(df.time.dt.hour
   .value_counts()
   .sort_index()
   .plot.bar(rot=0, color='k', width=.8))

pas = df[df.beer_style.str.lower().str.contains("pale ale")]
```

## Groupby

groupby about doing an operation on many subsets of the data, each of which shares something in common. The components of a groupby operation are:

**Components of a groupby**

1. **split** a table into groups
2. **apply** a function to each group
3. **combine** the results into a single DataFrame or Series



In pandas the `split` step looks like

```
df.groupby( grouper )
```

`grouper` can be many things

- Series (or string indicating a column in `df`)
- function (to be applied on the index)
- dict : groups by *values*
- `levels=[ names of levels in a MultiIndex ]`



`df.groupby(<name>)[<?selector>].agg(<combine function>)` or with standard functions like `.var()`,`.std()`,`.mean()` etc or with `.agg('mean')`



The output shape is determined by the grouper, data, and aggregation

Grouper: Controls the output index
single grouper -> Index
array-like grouper -> MultiIndex
Subject (Groupee): Controls the output data values
single column -> Series (or DataFrame if multiple aggregations)
multiple columns -> DataFrame
Aggregation: Controls the output columns
single aggfunc -> Index in the colums
multiple aggfuncs -> MultiIndex in the columns (Or 1-D Index if groupee is 1-D)

## Transform

A *transform* is a function whose output is the same shape as the input.

Just like `.agg` informs pandas that you want `1 input group → 1 output row`, the `.transform` method informs pandas that you want `1 input row → 1 output row`

`normalized = df.groupby("profile_name")[review_cols].transform(<function>)`

just like we want to apply function in a group region rather than a global region when compared to ‘function(df)’



# Tidy Data

In a tidy dataset:

1. Each variable forms a column
2. Each observation forms a row

Whether or not your dataset is tidy depends on your question

## Melt

- Collect a variable spread across multiple columns into one, but
- Repeat the metadata to stay with each observation

```python
tidy = pd.melt(games.reset_index(),
               id_vars=['game_id', 'date'], value_vars=['away_team', 'home_team'],
               value_name='team', var_name='home_away').sort_values(['game_id', 'date'])
# use 'game_id' and 'date' for melting, keep 'team' value untouched, melt into 'home_away' value
```

## Pivot_table

You can "invert" a `melt` with `pd.pivot_table`

```python
by_game = (pd.pivot_table(tidy, values='rest',
                          index=['game_id', 'date'],
                          columns='home_away')
             .rename(columns={'away_team': 'away_rest',
                              'home_team': 'home_rest'})
             .rename_axis(None, axis='columns'))
```

## Stack / Unstack

- stack: `DataFrame` -> `Series` with `MultiIndex`
- unstack: `Series` with `MultiIndex` -> `DataFrame`

The exact shape of a tidy dataset depends on the question being asked. Additionally, not all APIs expect tidy data, so you need to convert between "wide" and "long" form data.

# Time Series

- `pd.Timestamp` (nanosecond resolution `datetime.datetime`)
- `pd.Timedelta` (nanosecond resolution `datetime.timedelta`)

## Resampling

Resampling is similar to a groupby, but specialized for datetimes. Instead of specifying a column of values to group by, you specify a `rule`: the desired output frequency

`df.resample('MS').sum()`

## Rolling

Applying functions to windows, moving through your data.These are very similar to groupby and resample

## Time Offset

There are a whole bunch of offsets available in the `pd.tseries.offsets` namespace

```pyth
dates = pd.date_range("2016-01-01", end="2016-12-31", freq='D')

dates + pd.tseries.offsets.BDay(3)

dates + pd.tseries.offsets.MonthEnd()
```

## Timedelta Math

1. first use `pd.to_timedelta()` to convert data type
2. then calculate the date

# Large Data with Dask

Dask dataframe lets you work with larger than memory datasets. Dask breaks large problems into many small problems (task graph). It then executes those small problems in parallel and in a small memory footprint (scheduler)

```python
import numpy as np
import pandas as pd
import dask.dataframe as dd

df = dd.from_pandas(
    pd.DataFrame({'A': np.random.choice(['a', 'b', 'c'], size=100),
                  'B': np.random.randn(100),
                  'C': np.random.uniform(size=100)}),
    npartitions=4
)
# dask dataframes has most of the same methods as pandas
​```
Dask DataFrame's methods are lazy. This lets Dask build up a large chain of operations that can be executed in parallel.When you're ready for a concrete result, call compute.
​```
df[['B', 'C']].sum().compute()
```



# Useful function

1. df.value_counts()
2. df.sort_values()
3. df.pct_change() returns the `(current - previous) / previous` for each row 
4. df.diff() takes the value in the current row minus the value in the previous
5. df.squeeze() to convert 1 column df to a Series