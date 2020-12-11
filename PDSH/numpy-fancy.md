## Numpy Fancy

# Set Display Variables 

1. Maximum Width Columns

`pd.options.display.max_colwidth = 100`
`pd.set_option('display.max_colwidth', None)` - Multi Line Printing

2. Maximum Rows Shown

`pd.set_option('display.max_rows', 500)`

2. Temporary Setting

```
from IPython.display import display
with pd.option_context('display.max_rows', 100, 'display.max_columns', 10):
    display(df)
```

3. Reset Option

`pd.reset_option('display.max_rows')`
`pd.reset_option('all')`

# Tricks

1. at() method for Ufuncs

	`i = [2, 3, 3, 4, 4, 4]`
	`x = np.zeros(10)`
	`np.add.at(x, i, 1)`           [1 is added at indexes i, in X, and repetitions allowed in i]
	`print(x)` -> `[ 0.  0.  1.  2.  3.  0.  0.  0.  0.  0.]`

2. Histogramming

	`i = np.searchsorted(bins, x)`
	`np.add.at(counts, i, 1)`

# Sections

1. Broadcasting

	Broadcasting rules apply to any binary ufunc

	`M + a`
	`M + a[:,np.newaxis]`

	Broadcasting in NumPy follows a strict set of rules to determine the interaction between the two arrays:

		- Rule 1: If the two arrays differ in their number of dimensions, the shape of the one
		          with fewer dimensions is padded with ones on its leading (left) side.
		- Rule 2: If the shape of the two arrays does not match in any dimension, the array with
		          shape equal to 1 in that dimension is stretched to match the other shape.
		- Rule 3: If in any dimension the sizes disagree and neither is equal to 1, an error is raised.

2. Boolean Arrays

	Comparision Ufuncs (Outputs Boolean Arrays)

	==	`np.equal`
	!=	`np.not_equal`
	<	`np.less`
	<=	`np.less_equal`
	>	`np.greater`
	>=	`np.greater_equal`

	`np.any`/`np.all` aggregates can be used, output True/False
	All other aggreagates output Integers, Input Boolean arrays are converted as - [True - 1, False - 0]

	&	`np.bitwise_and`
	|	`np.bitwise_or`
	^	`np.bitwise_xor`
	~	`np.bitwise_not`

	`and` `or`        [ perform a single Boolean evaluation on an entire object ] 

	`&` `|`           [ perform multiple Boolean evaluations on the content 
	                        (the individual bits or bytes) of an object ]

3. Masking

	`x[x < 5]`        [Returns a 1D array filled with all the values that meet this condition]

	` np.median(inches[rainy & ~summer]))`

4. Fancy Indexing

	When using fancy indexing, the shape of the result reflects the shape of the index arrays rather than the shape of the array being indexed


	x - [51 92 14 71 60 20 82 86 74 74]

	`ind = np.array([[3, 7],[4, 5]])`
    `x[ind]`

    Answer `array([71, 86, 60])`

    X- [[ 0  1  2  3]
        [ 4  5  6  7]
        [ 8  9 10 11]]

    `row = np.array([0, 1, 2])`
	`col = np.array([2, 1, 3])`
	`X[row, col]`

	Answer - `array([ 2,  5, 11])`
	
	The first value in the result is X[0, 2], the second is X[1, 1], and the third is X[2, 3].

	Fancy Indexing also Follows Broadcasting Rules,

	`X[row[:, np.newaxis], col]`

	Answer - `array([[ 2,  1,  3], [ 6,  5,  7], [10,  9, 11]])`

	This can be combined with other Indexing schemes, 

	- simple indices       `X[2, [2, 0, 1]]` -> `array([10,  8,  9])`
	
	- slices               `X[1:, [2, 0, 1]]`  -> `array([[ 6,  4,  5], [10,  8,  9]])`
	
	- Boolean masks        `mask = np.array([1, 0, 1, 0], dtype=bool)`
	                       `X[row[:, np.newaxis], mask]`

	                       ->  `array([[ 0,  2], [ 4,  6], [ 8, 10]])`

5. Sorting

	`np.sort(x)`                      [Sorting]
	`x.sort()`                        [Inplace Sorting]
	`i = np.argsort(x)` and `x[i]`    [Argument Sorting]
	`np.sort(X, axis=0)`              [Sort along Column]

	`np.partition(x, 3)`              [First 3 entires are the smallest 3 numbers, others in other partition, unordered inside partition]
	`np.partition(X, 2, axis=1)`      [Partition along rows]
	`np.argpartition`                 [Gets index of partition]

6. Structured Data

	`data = np.zeros(4, dtype={'names':('name', 'age', 'weight'), 'formats':('U10', 'i4', 'f8')})`

	Can access using index/name or both, and can use boolean-masking, fancy-indexing, slicing.

	`data['name']` `data[0]` `data[-1]['name']` `data[data['age'] < 30]['name']`

	Compound Matrix types - `tp = np.dtype([('id', 'i8'), ('mat', 'f8', (3, 3))])`

	`data_rec = data.view(np.recarray)`         [makes it attributes instead of dictionary keys]

	`data['age']`  -> `data_rec.age`
