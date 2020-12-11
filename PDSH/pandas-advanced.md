## Pandas Advanced

# Contents

1. Aggregation and Grouping

	

2. Pivot Table



3. Scaling to Large Datasets

	`eval()`
	`query()`

4. Handling Pythonic Lists Inside

# Tricks

1. Assert Arrays

	Equal even if values a little different

	`np.allclose(mask, mask_numexpr)`

	Checks for both shape and values, also NaNs are equal

	`np.testing.assert_allclose`
	`np.testing.assert_array_almost_equal`

2. Check size of a Dataframe `df.values.nbytes`

3. memory Usage in MB `print(appended_data.memory_usage(deep=True).sum()/(1024*1024))`

4. Assert type 

	If an entry in a dataframe is a list (or any of these can be used- float, int, str, list, dict, tuple)

	Testing if it is a list of dicts

		`isinstance(dataframe_files.licenses.iloc[2],list)`      -> True
		`isinstance(dataframe_files.licenses.iloc[2][0],dict)`   -> True

# Sections

1. Aggregation and Grouping

	`ser = pd.Series(rng.rand(5))`
	`df = pd.DataFrame({'A': rng.rand(5), 'B': rng.rand(5)})`
	
	For Series, aggregations give a value: `ser.sum()` and `ser.mean()`.

	For DataFramaes, by **Default** the aggregates returns results by each **Column**: 

		`df.mean()`                     [ Gives two values for A and B ]

	For giving results Row-wise, we have to do 

		`df.mean(axis='columns')`       [ Here axis stands for how the result will be, means comp on other axis ]

	Convenience Function (Only Gives Numerical Values)

		`planets.dropna().describe()`

	Other Aggregations (Also all aggregations in Numpy)

		count()				Total number of items
		first(), last()		First and last item
		mean(), median()	Mean and median
		min(), max()		Minimum and maximum
		std(), var()		Standard deviation and variance
		mad()				Mean absolute deviation
		prod()				Product of all items
		sum()				Sum of all items


	Group By

		`df.groupby('key')`              [ Returns a View ]
		`df.groupby(df['key'])`          [ Does the same thing ]

	Apply Aggregates/Any Pandas Function on this group

		`df.groupby('key').sum()`

	Groupby Methods Indexed the same way as Dataframes

		`planets.groupby('method')['orbital_period']`

	This performs aggregations on one axis - `orbital_period`, while grouped on another `method`

		`planets.groupby('method')['orbital_period'].median()`

	Supports iterations over groups

	```
	for (method, group) in planets.groupby('method'):
    print("{0:30s} shape={1}".format(method, group.shape))
	```

	Dispatch Methods

		`planets.groupby('method')['year'].describe()`     [ For all `method` groups performs aggregations on `year` ]

	Aggregation of Methods on Groups

		`df.groupby('key').aggregate(['min', np.median, max])`              [Results in MultiIndex]
		`df.groupby('key').aggregate({'data1': 'min', 'data2': 'max'})`

	Filter using Defined Functions   [ Drops a whole group based on STD values of a column ]
	The filter function should return a Boolean value specifying whether the group passes the filtering.

		```
		def filter_func(x):
			return x['data2'].std() > 4

		df.groupby('key').filter(filter_func)
		```

	Some transformation of the data

		`df.groupby('key').transform(lambda x: x - x.mean())`

	Apply some arbitrary function to group results

		```
		def norm_by_data2(x):
		    # x is a DataFrame of group values
    		x['data1'] /= x['data2'].sum()
    		return x

    	df.groupby('key').apply(norm_by_data2)
		```

	`apply()` within a `GroupBy` is quite flexible: the only criterion is that the function takes a DataFrame
	and returns a Pandas object or scalar; what you do in the middle is up to you!

	Other Methods of Grouping - 

		- Passing a list of the same length

			`L = [0, 1, 0, 1, 2, 0]`                 [ Length is same as the number of rows ]
			`display('df', 'df.groupby(L).sum()')`   [ Grouped into the unique values of L ]

		- Passing a dictionary from Index to Groups

			`mapping = {'A': 'vowel', 'B': 'consonant', 'C': 'consonant'}`
			`df2.groupby(mapping).sum()`

		- Passing a python function that takes in Index, gives out group

			`df2.groupby(str.lower).mean()`          [ Takes in `A`, gives out `a` ]

		- Any of the preceding choices can be combined into a multi-index (unstack() to send one Index to column)

			`df2.groupby([str.lower, mapping]).mean()`   [ One Index - a - vowel ]

	Super Example

		`planets.groupby(['method', decade])['number'].sum().unstack().fillna(0)`


2. Pivot Tools

	Pivot tables are essentially a multidimensional version of GroupBy aggregation.
	That is, you split-apply-combine, but both the split and the combine happen across
	not a one-dimensional index, but across a two-dimensional grid.

	Pivot Tables using Groups

		`titanic.groupby('sex')[['survived']].mean()`                                [ Series with mean values per sex ]
		`titanic.groupby(['sex', 'class'])['survived'].aggregate('mean').unstack()`  [ Df with mean values per sex and class ]

	Pivot Tables for the same

		`titanic.pivot_table('survived', index='sex', columns='class')`              [ Default - Mean Aggregate ]

	Both Index and Columns can be Multi-Level, given by a list of tuples.

		`age = pd.cut(titanic['age'], [0, 18, 80])`              [ Equal Bins if not given explicitely ]
		`fare = pd.qcut(titanic['fare'], 2)`                     [ Quantile Bins ]

		`titanic.pivot_table('survived', ['sex', age], [fare, 'class'])`

	We can also control what types of Aggregations are made, on even Multiple Columns [ Values omitted ]

		`titanic.pivot_table(index='sex', columns='class', aggfunc={'survived':np.sum, 'fare':'mean'})`

	Add `margines` = True for total of rows and columns

		`titanic.pivot_table('survived', index='sex', columns='class', margins=True)`

3. Performance `eval()` and `query()`

	Evaluates directly without intermediate values

		`import numexpr`
		`mask_numexpr = numexpr.evaluate('(x > 0.5) & (y < 0.5)')`

	Using the `eval()` function

		`df1 + df2 + df3 + df4`                    # 82.9 ms
		`pd.eval('df1 + df2 + df3 + df4')`         # 26.3 ms and much less memory

	`eval` supports

		- `pd.eval('-df1 * df2 / (df3 + df4) - df5')`                 [ All arithmetic operations ] 
		- `pd.eval('df1 < df2 <= df3 != df4')`                        [ All comparision operators ]
		- `pd.eval('(df1 < 0.5) & (df2 < 0.5) | (df3 < df4)')`        [ All bitwise operators ]
		- `pd.eval('(df1 < 0.5) and (df2 < 0.5) or (df3 < df4)')`     [ Literal and or operators ]
		- `pd.eval('df2.T[0] + df3.iloc[1]')`                         [ Object attributes and Indices ]
		- Math functions: sin, cos, exp, log, etc.

		Column Wise Operations

		- `pd.eval("(df.A + df.B) / (df.C - 1)")`        [ df.A -> df['A'] ]
		- `df.eval('(A + B) / (C - 1)')`                 [ Does the same ]

		Assignment/Modification

		- `df.eval('D = (A + B) / C', inplace=True)`     [ Makes a new column D, or modifies ]
		
		Local Variables

		- `df.eval('A + @column_mean')`               [ Not `pd.eval` ]

	Query Method	

		- `pd.eval('df[(df.A < 0.5) & (df.B < 0.5)]')`
		- `df.query('A < 0.5 and B < 0.5')`

		This also takes in local variables

		- `df.query('A < @Cmean and B < @Cmean')`

4. Handling Pythonic Objects inside Dataframes

	Explode List

	`df.explode('column_name')`          [Explodes by the `column_name`]

	Explode means, if that column has

	If the `ratings` column in the dataframe is a list of dicts,

		1. Makes multiple rows for each dict item in the list, grouped by `movie_id`
		2. Makes each value in the dict a column 

			`parsed = x.groupby('movie_id').ratings.apply(lambda x: pd.DataFrame(x.values[0])).reset_index()`

	One column has dicts inside, has to select by key and convert to DataFrame Columns [Same Rows]

		```
		def dict_to_rows_in_dataframes(dataframe):
    		row_data = dataframe['content']['license_clarity_score']
    		return row_data

		dataframe_dicts = data_json.json_content.apply(dict_to_rows_in_dataframes)
		new_df = pd.DataFrame(list(dataframe_dicts))
		old_df.merge(new_df)
		```

5. Assertion Techniques

