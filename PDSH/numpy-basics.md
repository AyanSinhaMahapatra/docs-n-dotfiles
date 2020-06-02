## Numpy Basics

# Tricks

1. View/Copy Checking
   `print (np.may_share_memory(a, b))`
   `print (b.base is a)`
   `print (b.flags['OWNDATA'])`

2. Random States

	`rng = np.random.RandomState(0)`     [Makes a local Random State doesn't effect the global]
	`x = rng.randint(10, size=(3, 4))`   [Has to be used explicitely]
	`r.get_state()`

	`np.random.seed(1234)`               [Global Randomstate declaration for all random # generation]
	`np.random.uniform(0, 10, 5)`
	`np.random.get_state()`

# Sections/Topics
 
1. Fill Arrays 

    `np.array([3.14, 4, 2, 3])`                       [All up-casted to floating points]
    `np.array([range(i, i + 3) for i in [2, 4, 6]])`
    `np.array([1, 2, 3, 4], dtype='float32')`         [int8-int64,same uint,float16-64,complex64-128]

2. Types of Arrays

	`np.zeros(10, dtype=int)`
	`np.ones((3, 5), dtype=float)`
	`np.full((3, 5), 3.14)`            [All values 3.14, size - 3,5]
	`np.eye(3)`                        [Identity Matrix]
	`np.empty(3)`                      [Uninitialized]
	`np.zeros_like(bins)`              [Zeros in the same shape]
	`np.diagonal(x)`                   [Prints diagonal of the Matrix X]

	`np.arange(0, 20, 2)`              [From 0-20, spaced 2, like 0,2,4..., Start Def 0]
	`np.linspace(0, 1, 5)`             [Range and Number of Values, evenly spaced]

	`np.random.random((3, 3))`         [uniformly distributed random float bet 0-1]
	`np.random.randint(0, 10, (3, 3))` [uniformly distributed random integer bet 0-10]
	`np.random.normal(0, 1, (3, 3))`   [Mean 0, SD 1, Normal Distrib]

	`np.pi`                            [Value of Pi]

3. Indexing

	`x3[0,1,1]` OR `x3[0][1][1]`       [Zero Indexed]
	`x3[0,1,-1]`                       [Negative means from back, starts at 1]
	`x3[0,0,0] = 3.14`                 [Trancated if inputing Float into Int arraay]

4. Slicing (**Returns Views and Not Copies**)

	`x[start:stop:step]`               [From Start (included), Till stop (excluded), 
	                                    step (negative if reversed)]
    `x2[:3, ::2]`                      [Multi-dim slicing]
    `x2_copy = x2[:2, :2].copy()`      [Copying a sliced view]

5. Reshaping (**Tries to return Views and Not Copies**)
	
	`grid = np.arange(1, 10).reshape((3, 3))`  [Reshaping must match size]
	
	`x = np.array([1, 2, 3])`
	`x[np.newaxis, :]` and `x[:, np.newaxis]`  [row and column vector via newaxis]

6. Concatenation

	`np.concatenate([grid, grid], axis=1)`     

	   This can be any number of arrays, or dimensions (upto 32) and the inputs have to have that dim
	   Default axis = 0
	   If axis = None, Arrays are flattened before concat
	   All the input array dimensions except for the concatenation axis must match exactly (valid for all below)

	`np.stack`          [Stacks into a new dim at axis 0, others shifted 1]
	`np.vstack`         [Stacks along axis 0]
	`np.hstack`         [Stacks along axis 1]
	`np.dstack`         [Stacks along axis 2]

7. Splitting
	
	`x1, x2, x3 = np.array_split(x, 3)`
	`x1, x2, x3 = np.array_split(x, [3,7], axis=1)`

	Splits into 0 - 2, 3 - 6, 7 - 9, along axis 1.

	Identical to `np.split`, but won't raise an exception if the groups aren't equal length.
	[In case of just no of splits given]
	
	`np.vsplit`
	`np.hsplit`
	`np.dsplit`

8. Ufuncs

	Basics

	+	`np.add`
	-	`np.subtract`
	-	`np.negative`
	*	`np.multiply`
	/	`np.divide`
	//	`np.floor_divide`
	**	`np.power`
	%	`np.mod`

	`np.absolute(x)`/`np.abs(x)`    [Can also convert Complex Numbers]

	Trigonometric/Log/Exponents

	`np.sin(theta)` `np.cos(theta)` `np.tan(theta)` `np.arcsin(x)` `np.arccos(x)` `np.arctan(x)`
	`np.exp(x)` `np.exp2(x)` `np.power(3, x)` 
	`np.log(x)` `np.log2(x)` `np.log10(x)`
	`exponent = np.log(array) / np.log(base)`   [Log of base n]

	Special

	`from scipy import special`
	`.gamma(x)` `.gammaln(x)` `.beta(x, 2)` `.erf(x)` `.erfc(x)` `.erfinv(x)`

	Advanced UFuncs

	`np.power(2, x, out=y[::2])`    [Specify output Locations for Large Outputs]

	`np.add.reduce(x)`              [ Adds all ]
	`np.add.accumulate(x)`          [ Stores intermediate results ]               
	`np.add.outer(x, x)`            [Performs all pairwise calculations]
	                                [These 3 are valid for all binary operations]

9. Aggregates 

	`M.sum()`              [By default sums over entire multiple dimensions]
	`M.min(axis=0)`        [Aggregate by axis]
	`M.max(axis=1)`        [if M is (2,3,4,5) the result is (2,4,5)]

	Agrregates and their NaN safe versions

	np.sum			np.nansum
	np.prod			np.nanprod
	np.mean			np.nanmean
	np.std			np.nanstd
	np.var			np.nanvar
	np.min			np.nanmin
	np.max			np.nanmax

	np.argmin		np.nanargmin
	np.argmax		np.nanargmax
	np.median		np.nanmedian       `np.median(heights)`
	np.percentile	np.nanpercentile   `np.percentile(heights, 25)`

	np.any		Evaluate whether any elements are true
	np.all      Evaluate whether all elements are true

# ToDo 

1. Read reshape docs, when copy when not - `np.reshape?`