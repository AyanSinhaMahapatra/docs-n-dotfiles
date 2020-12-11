## Pandas Basics

# Tricks

1. 

# Sections

1. Series Object

	`data = pd.Series([0.25, 0.5, 0.75, 1.0], index=['a', 'b', 'c', 'd'])`

		- Index can be any type (defaults to an int seq starting from 0)
		- can be non-contiguous or non-sequential
		- All indexing schemes [Masking, Fancy-Indexing, Slicing] available
		- Being non-numerical/non-sequential deosn't effect indexing schemes

	`data.values` -> `array([ 0.25,  0.5 ,  0.75,  1.  ])`  [Numpy Array]
	`data.index` ->  `RangeIndex(start=0, stop=4, step=1)`

	`I = np.array(['a','b'])` -> `data[I]`    [Fancy Indexing]
	`index=[2, 5, 3, 7]` -> `data[2:7]`       [Slicing]
	`data[data.values > 0.5]`                 [Masking]

	`'a' in data`             [ Examine if in the Series Object ]
	`data.keys()`             [ Shows Indexes ]
	`list(data.items())`      [ Shows Indexes and Values as an Iterable ]
	`for i in data.items()`   [ Iterate through ]

	As a dictionary (Also supports all indexing schemes)

		`population_dict = {'California': 38332521,'Texas': 26448193,'New York': 19651127}`
		`population = pd.Series(population_dict)`

	Indexing Schemes for Dictionaries

		`population['California']`
		`population['California':'New York']`
		`population[population.values == 38332521]`

	`population["Los Angeles"] = 1374687`    [ Extend by new value ]

	`pd.Series(data, index=index)` data can be 

		- list    - numpy-array  - scalar(repeated to fill indexes)  - dictionary

	`pd.Series({2:'a', 1:'b', 3:'c'}, index=[3, 2])`    
	[1:'b' is deleted as not in explicitely identified keys]

	Explicit/Implicit Indexing
	
		data = pd.Series(['a', 'b', 'c'], index=[1, 3, 5])

	Normally

		`data[1] -> a` is handled explicitly, and 
		`data[1:3] -> 'b,c'` is handled implicitly (Using python indexing and not explicit)

	Always Explicit - `data.loc[1]`
	Always Implicit - `data.iloc[1]`

2. Dataframe Object

	`area_dict = {'California': 423967, 'Texas': 695662, 'New York': 141297,`
	`area = pd.Series(area_dict)`

	`states = pd.DataFrame({'population': population, 'area': area})`    
	[Creating a DataFrame from Two series Objects]

	`states.index` and `states.columns` gives Indexobjects with indexes and columnnames.

	`states.values` gives a 2D array

	`states['area']` and `states.area` both giver individual Series Objects i.e columns.
		
		(If the column names are not strings, or if the column names conflict with methods of the DataFrame,
		this attribute-style access is not possible)

	`states['area']['Texas']` gives individual values.

	Can be created by 

		- From a single Series object `pd.DataFrame(population, columns=['population'])`
		- List of Dicts  `data = [{'a': i, 'b': 2 * i} for i in range(3)]`   [Using List Comprehension]
		- Dictionary of Series Objects `pd.DataFrame({'population': population, 'area': area})`
		- 2D Numpy array `pd.DataFrame(np.random.rand(3, 2), columns=['foo', 'bar'], index=['a', 'b', 'c'])`
		- Structured Numpy Array `A = np.zeros(3, dtype=[('A', 'i8'), ('B', 'f8')])` -> `pd.DataFrame(A)`

	`pd.DataFrame([{'a': 1, 'b': 2}, {'b': 3, 'c': 4}])`   If some keys missing, filled with NaN

	Attributes - `ind.size`, `ind.shape`, `ind.ndim`, `ind.dtype`

	Like Series Object, Dictionary Style Syntax for accessing and adding new columns are valid.

		`data['density'] = data['pop'] / data['area']`

	Dataframe as a 2D array

		`data.values`
		`data.T`
		`data.values[:,0]`

	Implicit/Explicit Indexing like Series Objects

		`data.iloc[:3, :2]`                  [Implicit]
		`data.loc[:'Illinois', :'pop']`      [Explicit]

	Two types of indexing can even be combined like this:

		`data.loc[data.density > 100, ['pop', 'density']]`

	Data modification

		`data.iloc[0, 2] = 90`

	Indexing Conventions in DataFrames
	
		`data['density']`               [Indexing refers to columns]

		`ata[['density','pop']]`        [Fancy Indexing also refers to columns]

		`data['Florida':'Illinois']`	[Slicing refers to rows]
		`data[1:3]`                     [Same but using numbers]

		`data[data.density > 100]`      [Direct maskings are also Row-wise]

	Data Types of all columns

		`df_1.dtypes`

	Memory Usage of a DataFrame

		`appended_data.memory_usage(deep=True)`                            [ Bytes of Space per column ]
		`print(appended_data.memory_usage(deep=True).sum()/(1024*1024)) `  [ Total Space in MB ]

	Change an Column to an Index

		`df.set_index('sha1', inplace=True)`

3. Index Object

	`ind = pd.Index([2, 3, 5, 7, 11])`       [From scratch]
	`ind = states.index`                     [From existing Dataframe/Series]

	`indA | indB`                            [Union]
	`indA & indB`                            [Difference]
	`indA ^ indB`                            [Symmatric Difference]

4. Operations on Dataframes and Series

	Ufuncs Work on Dataframes and Series

		`np.exp(ser)`
		`np.sin(df * np.pi / 4)`

	Indexes are aligned, when performing binary operations (i.e. missing indexes are ignored)

		`population / area`
		`area.index | population.index`          [Resulting array contain values for these indexes]

		(With NaN for the values where indexes don't exist)
		`A / B`                         [This will fill up values as NaN]
		`A.divide(B, fill_value=0)`     [This will populate all missing values by 0, and then perform operation]

		[Division by 0 here results in 'inf', if value is inside Series/Dataframe Object]

	In DataFrames, Indices are aligned correctly irrespective of their order in the two objects, and indices in the result are sorted. (Unnecessary Operation)

	Broadcasting Rules also apply like as in Numpy (One row is broadcasted rowwise) 

		`df = pd.DataFrame(rng.randint(10, size=(3, 4)), columns=list('QRST'))`
		`df - df.iloc[0]`

	For column wise broadcasting,

		`df.subtract(df['R'], axis=0)`

	These will automatically align indices between two elements	

		`halfrow = df.iloc[0, ::2]`      [ This selects out one row, and 2/4 columns ]
		`df - halfrow`                   [ This is performed only along those 2 columns, others are NaN ]

5. Handling Missing Data

	`None` 

		It is a Pythonic Object, so it can only be in Numpy/Pandas arrays with dtype `object`,
		So operations will be Python Level and slow.

		Also if aggregations like `sum()` are performed then it will give an Error.

	`NaN` -> `np.nan`

		This is a special floating point value, abbreviated like `Not a Number`.
		Regardless of the operation, the result of arithmetic with `NaN` will be another `NaN` [Data Virus].

		So normal aggregates don't give errors, but generate `NaN`.

			`vals2 = np.array([1, np.nan, 3, 4]) `
			`vals2.sum(), vals2.min(), vals2.max()` -> `(nan, nan, nan)`

		So there's special NaN-safe functions (Those ignore the missing Values) (Same Performance)

			`np.nansum(vals2), np.nanmin(vals2), np.nanmax(vals2)`

	When data is loaded in a series, both `None` and `nan` converted to -> `nan`.
	Also, integer array type-casted to Floating Point if a `nan`/`None` value is given in input.

	[In pandas string datatype is always stored as object]

	Operating on Null values (Series)

		`data.isnull()`            [ Creates booloan array, `True` at `nan`/`None`, `False` elsewhere ]
		`data.notnull()`           [ Opposite of `isnull` ]
		`data[data.notnull()]`     [This would drop values from Series but not Dataframes]
		`data.dropna()`            [Gives the same thing as above]

	We cannot drop single values from a DataFrame; we can only drop full rows or full columns.

		`df.dropna()`                [ Will drop any row where a null value is present ]
		`df.dropna(axis='columns')`  [ Will drop any column where a null value is present ]

	Now, by default if any `nan` is present it is deleted, but we can change this.

		`df.dropna(how='all')`                [ Drops only if all values in a row are `NaN` ]
		`df.dropna(axis='rows', thresh=3)`    [ Minimum non-null values for the row to be kept ]

	We can also populate a value in the `NaN` values,

		`data.fillna(0)`                [ Fills zero ]
		`data.fillna(method='ffill')`   [ Fills with the value right after it ]
		`data.fillna(method='bfill')`   [ Fills with the value right before it ]

	It's same for DataFrames, 

		`df.fillna(method='ffill', axis=1)`      [ Fills along columns ] 

