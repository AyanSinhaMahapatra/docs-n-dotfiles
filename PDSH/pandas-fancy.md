## Pandas Basics

# Tricks

1. Reindex a DataFrame - `pop = pop.reindex(index)`

2. Display HTML representation of multiple objects

```
class display(object):

    template = """<div style="float: left; padding: 10px;">
    <p style='font-family:"Courier New", Courier, monospace'>{0}</p>{1}
    </div>"""
    def __init__(self, *args):
        self.args = args
        
    def _repr_html_(self):
        return '\n'.join(self.template.format(a, eval(a)._repr_html_())
                         for a in self.args)
    
    def __repr__(self):
        return '\n\n'.join(a + '\n' + repr(eval(a))
                           for a in self.args)
```

# Sections

1. Hierarchical Indexing (MultiIndex)

	Bad Way

		`index = [('California', 2000), ('California', 2010)]`
		`populations = [33871648, 37253956]`
		`pop = pd.Series(populations, index=index)`

	And access like:

		`pop[('California', 2010):('Texas', 2000)]`
		`pop[[i for i in pop.index if i[1] == 2010]]`

	MultiIndex

		`index = pd.MultiIndex.from_tuples(index)`

		Looks like:

		California  2000    33871648
		            2010    37253956
		New York    2000    18976457
		            2010    19378102
		dtype: int64

	Access Operations:

		`pop[:, 2010][2]`      [ Gives a value ]
		`pop[:, 2010]`         [ Gives a Series ]

	The years can be converted as Rows and vice-versa in the following manner,
	
		`pop_df = pop.unstack()`       [ Makes the [years] as Columns ]
		`pop_df.stack()`               [ Same as `pop` ]	

	In case of Multiple Indexes, use `level` like `pop.unstack(level=1)`.

		`pop.unstack()`                [ Has default `level`=1, i.e. here the years ]
		`pop.unstack(level=0)`         [ Converts the names like `Texas` to columns ]

	Same can be used to perform Aggregations

		`data_mean.mean(axis=1, level='type')`

	Add another column

		`pop_df = pd.DataFrame({'total': pop, 'under18': [9267089, 9284094]})`


	Methods of MultiIndex Creation:

		- List of two or more index arrays (All index arrays must be of same length)

			df = pd.DataFrame(np.random.rand(4, 2),
			                index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]], columns=['data1', 'data2'])

		- Dictionary with appropriate tuples as keys

			ser_mul = pd.Series({('California', 2000): 33871648,('California', 2010): 37253956,})

	Explicit Methods:

	All these create this same MultiIndex object

	`MultiIndex([('a', 1),('a', 2),('b', 1),('b', 2)],)`

		- `pd.MultiIndex.from_arrays([['a', 'a', 'b', 'b'], [1, 2, 1, 2]])`

		- `pd.MultiIndex.from_tuples([('a', 1), ('a', 2), ('b', 1), ('b', 2)])`

		- `pd.MultiIndex.from_product([['a', 'b'], [1, 2]])`      [Like a cartesian product]

		- `pd.MultiIndex(levels=[['a', 'b'], [1, 2]], codes=[[0, 0, 1, 1], [0, 1, 0, 1]])`

		- From Dataframe (ToDo)

	Set/Change MultiIndex Level names

		`pop.index.names = ['state', 'year']`
		`pop.columns.names = ['Subject','Type']`

	In DataFrames the columns can also be set as MultiIndex

		`health_data = pd.DataFrame(data, index=index, columns=columns)`   [Where index,columns are MultiIndex]

	Partial Slicing is available if MultiIndex is sorted.

		`data = data.sort_index()`            [ Sorts all Index levels ]
		`pop.loc['California':'New York']`

	All other schemes of indexing also possible.

	Indexing in 4d MultiIndex

		`health_data['Guido', 'HR'][2013:,1]`        [ Gives value in 4d multiindex DataFrame]
		`health_data.loc[:, ('Bob', 'HR')]`
		`health_data.iloc[:2, :2]`

	Trying to create a slice within a tuple will lead to a syntax error

		`health_data.loc[(:, 1), (:, 'HR')]`   ->  Error
		`idx = pd.IndexSlice`
		`health_data.loc[idx[:, 1], idx[:, 'HR']]`  ->  Correct

	Rearrange hierarchical data is to turn the index labels into columns

		`pop_flat = pop.reset_index(name='population')`
		`pop_flat.set_index(['state', 'year'])`

	Data Aggregations on Multi-Index

		 `data_mean.mean(axis=1, level='type')`     

2. Concat and Append

	Concatenation along Series (Along the only axis)

		`pd.concat([ser1, ser2])`

	Concatenation of Dataframes

		`pd.concat([df1, df2])`               [ By default happens along the rows, i.e. axis=0 ]
		`pd.concat([df3, df4], axis=1)`

	Here the axis along which the concatenation is happening should have same indexes, or there'll be `NaN` values. Or, Inner Joins can be performed, having only common indexes.

		`pd.concat([df5, df6], join='inner')`                 [ Default = 'outer' ]

	Another option is append:

		`pd.concat([df1, df2])` -> `df1.append(df2)`
	
	Also along the other axis, repetations are allowed, unless specified like this:

		`pd.concat([x, y], verify_integrity=True)`

	Or indexes of both DataFrames can be ignored, and given a new integer series as index:

	    `pd.concat([x, y], ignore_index=True)`

	We can also make a Multi-Index out of two Simple Indexes, while concatination:

		`pd.concat([x, y], keys=['x', 'y'])`

3. Marge and Join

	This merges two Dataframes (Searches for common rows if not given)

		`df3 = pd.merge(df1, df2)`

	Joins can be (can be validated like) :

		- One-to-one [ All keys are distinct in the columns by which joined ]

		- Many-to-one [ Duplicate keys in left DataFrame in the coulumn by which joined, for every key, the 
		                second DataFrame data is repeated ] [ Similarly one-to-many ]

		- Many-to-many [ Duplicate keys in both DataFrames in the coulumn by which joined, for every key, the 
		                 DataFrame data is repeated, for both DataFrames, making a cartesian product ]

	Can be validated like:

		`df3 = pd.merge(df1, df2, validate="one_to_one")`

	Merge `On` -

		`pd.merge(df1, df2, on='employee')`          [Here both DataFrames must have the `employee` column ]

		`pd.merge(df1, df3, left_on="em", right_on="name")`    [ Merge is performed by these Columns specified ]

	If it's a Merge on Indexes, (or Join)

		`pd.merge(df1a, df2a, left_index=True, right_index=True)`
		`df1a.join(df2a)`                                            [Same as Above]

	`left_on`/`right_on` and `left_index`/`right_index` can be mixed together:

		`pd.merge(df1a, df3, left_index=True, right_on='name')`

	When a value appears in one key column but not the other, we need types of Join to specify using `how`:

	- inner (default)   - outer     - left     - right    [Other than inner, these generate NaN values]

		`pd.merge(df6, df7, how='outer')`

	If DataFrames have conflicting column names, he merge function automatically appends a suffix `_x` or `_y`

		`pd.merge(df8, df9, on="name", suffixes=["_L", "_R"])`    [This is how the defaults are changed]

	Sorting is done if `sort` is set to `True` (And in `outer` join).

	Setting `indicator` to `True` add column `_merge` with source information, whether the merge keys
	are present in :

	- `left_only`  - `right_only`  - `both`

	Check for `NaN` values and eliminate:

		`merged.isnull().any()`                               [ looking for rows having Nulls ]
		`merged[merged['population'].isnull()].head()`        [ which info are Null ]
		`merged.loc[merged['state'].isnull(), 'state/region'].unique()`    [ which regions has state null ]
		`merged.loc[merged['state/region'] == 'PR', 'state'] = 'Puerto Rico'`  [ Replace states with new abrv ]

4. Aggregation and Grouping

	`ser = pd.Series(rng.rand(5))`
	`df = pd.DataFrame({'A': rng.rand(5), 'B': rng.rand(5)})`
	
	For Series, aggregations give a value: `ser.sum()` and `ser.mean()`.

	For DataFramaes, by **Default** the aggregates returns results by each **Column**: 

		`df,mean()`                     [ Gives two values for A and B ]

	For giving results Row-wise, we have to do 

		`df.mean(axis='columns')`       [  ]