## Pandas Fancy

# Contents

1. MultiIndex


2. Concat Append

3. Merge Join

4. Vectorized String Operations

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

3. Set dtypes `births['day'] = births['day'].astype(int)`

4. 

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


4. Vectorized String Operations

	`data = ['peter', 'Paul', None, 'MARY', 'gUIDO']`
	`names = pd.Series(data)`

	Capitalize all Strings

		`[s.capitalize() for s in data]`      [ Fails at None ]
		`names.str.capitalize()`              [ Ignores None ]

	Python's Built in String Methods are mirrored in Pandas Vectorized String Methods

		len()		          [ Returns String length ]
		lower()			
		translate()		
		islower()
		ljust()		
		upper()			
		startswith('T')
		       [ Returns Boolean Values ]	
		isupper()
		rjust()		
		find()			
		endswith()		
		isnumeric()
		center()	
		rfind()			
		isalnum()		
		isdecimal()
		zfill()		
		index()			
		isalpha()		
		split()                [ Compound Values for each element, i.e. the words ]
		strip()		
		rindex()		
		isdigit()		
		rsplit()
		rstrip()	
		capitalize()	
		isspace()		
		partition()
		lstrip()	
		swapcase()		
		istitle()		
		rpartition()

	All of this are Called like - `pd.str.len()`

	Several Methods that accept Regular Expressions - 

		```
		match()		Call re.match() on each element, returning a boolean.
		extract()	Call re.match() on each element, returning matched groups as strings.
		findall()	Call re.findall() on each element
		replace()	Replace occurrences of pattern with some other string
		contains()	Call re.search() on each element, returning a boolean
		count()		Count occurrences of pattern
		split()		Equivalent to str.split(), but accepts regexps
		rsplit()	Equivalent to str.rsplit(), but accepts regexps
		```

	Miscellaneous

		```
		get()			Index each element
		slice()			Slice each element
		slice_replace()	Replace slice in each element with passed value
		cat()			Concatenate strings
		repeat()		Repeat values
		normalize()		Return Unicode form of string
		pad()			Add whitespace to left, right, or both sides of strings
		wrap()			Split long strings into lines with length less than a given width
		join()			Join strings in each element of the Series with passed separator
		get_dummies()	extract dummy variables as a dataframe
		```

	Pythonic Slicing and Indexing

		`df.str.slice(0, 3)` -> `df.str[0:3]`    [ Slices every string ]
		`df.str.get(i)`      -> `df.str[i]`      [ Gives out every i'th char ]

	These `get()` and `slice()` methods also let you access elements of arrays returned by `split()`.

		`monte.str.split().str.get(-1)`

	Indicators

		`full_monte = pd.DataFrame({'name': monte,'info': ['B|C|D', 'B|D', 'A|C','B|D', 'B|C', 'B|C|D']})`
		`full_monte['info'].str.get_dummies('|')`       [ This can be any character ]

		This gives a dataframe with 4 Columns A,B,C,D and only 0/1 Values on whther they were present.

	Uses Examples

		`recipes.ingredients.str.len().describe()`                 [ Aggregations on String Length ]
		`recipes.name[np.argmax(recipes.ingredients.str.len())]`   [ Longest Recipe ]
		`recipes.description.str.contains('[Bb]reakfast').sum()`   [ How many recipes mentions Breakfast ]

		```
		spice_list = ['salt', 'pepper', 'oregano', 'sage', 'parsley', 'rosemary',
		                                                        'tarragon', 'thyme', 'paprika', 'cumin']
		spice_df = pd.DataFrame(dict((spice, recipes.ingredients.str.contains(spice, re.IGNORECASE))
                                                                for spice in spice_list))
        ```

        Here `re.IGNORECASE` makes the search case insensitive. `spice_df` is a Boolean Dataframe.

        `selection = spice_df.query('parsley & paprika & tarragon')`
        `recipes.name[selection.index]`


5. Time Series

	Python DateTime

		`from datetime import datetime`  `datetime(year=2015, month=7, day=4)`
		`from dateutil import parser`    `parser.parse("4th of July, 2015")`

	Numpy `datetime64`

		`date = np.array('2015-07-04', dtype=np.datetime64)`
		`np.datetime64('2015-07-04 12:59:59.50', 'ns')`

	Pandas DateTime

		`date = pd.to_datetime("4th of July, 2015")`

	Pandas DateTime Functions

		`date.strftime('%A')`                          [ What day is it ]
		`date + pd.to_timedelta(np.arange(12), 'D')`   [ Vectorized Operations ]

	Indexing by Time
	
		`index = pd.DatetimeIndex(['2014-07-04', '2014-08-04', '2015-07-04', '2015-08-04'])`
		`data = pd.Series([0, 1, 2, 3], index=index)`
		`data['2014-07-04':'2015-07-04']`
		`data['2014']`

	TimeSeries Data Structures in Pandas

	- For time stamps, Pandas provides the `Timestamp` type. As mentioned before, it is essentially a replacement for
	  Python's native datetime, but is based on the more efficient `numpy.datetime64` data type. The associated Index
	  structure is `DatetimeIndex`.

	- For time Periods, Pandas provides the `Period` type. This encodes a fixed-frequency interval based on `numpy.datetime64`.
	  The associated index structure is `PeriodIndex`.
	
	- For time deltas or durations, Pandas provides the `Timedelta` type. `Timedelta` is a more efficient replacement for
	  Python's native `datetime.timedelta` type, and is based on `numpy.timedelta64`. The associated index structure
	  is `TimedeltaIndex`.

	`Timestamp` and `DatetimeIndex`

		Parsing all formats

			`dates = pd.to_datetime([datetime(2015, 7, 3), '4th of July, 2015', '2015-Jul-6', '07-07-2015', '20150708'])`

		`pd.date_range('2015-07-03', '2015-07-10')`
		`pd.date_range('2015-07-03', periods=8, freq='H')`
		`pd.period_range('2015-07', periods=8, freq='M')`

	Converting `DatetimeIndex` can be converted to a `PeriodIndex`

		`dates.to_period('D')`

	Creating `TimedeltaIndex`

		`dates - dates[0]`
