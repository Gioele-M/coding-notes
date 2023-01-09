# Linear algebra

Linear algebra focuses on the mathematics surrounding linear operations and solving systems of linear equations.
Linear algebra studies linear transformations, including the use of vectors and matrices. It allows us to solve problem like linear regression and problems concerning large amount of data.



## Vectors
Vectors are defined as quantities having both direction and magnitude, compared to scalar quantities that only have magnitude, and therefore consist of two or more elements of data. The dimensionality of a vector is determined by the number of numberical elements in that vector (eg. a vector with 4 elements has a dimensionality of 4)

Vectors are represented as a series of numbers enlosed in parentheses, angle brackets or square brackets
	[x]
v = [y]
	[z]

The magnitude (or length) of a vector _||v||_ can be calculated as the sum of each vector component squared

||v|| = sqrt(v1^2 + v2^2 + ...) 

Example:
A ball is travelling through the air given the velocieties x = -12, y = 8, z=-2. This can be converted to a vector and its velocity can be found through the magnitude of the vector eg:
||v|| = sqrt((-12)^2 + 8^2 + (-2)^2) = 14.56



### Vector operations

*Scalar multiplication*
Any vector can be multiplied by a scalar (constant), resulting in every element of that vector multiplied by that scalar individually
eg
 [x]	   [kx]
k[y] * m = [ky] 
 [z]	   [kz]



*Addition and subtraction*
Vectors can be added and subtracted from each other when they are of the same dimension. Vector addition is commutative (the order doesn't matter) and associative (we can swap parentheses around)

eg
[x1]	[x2]	[x3]   [x1 + 2x2 - 3x3]
[y1] + 2[y2] - 3[y3] = [y1 + 2y2 - 3y3]
[z1]	[z2]	[z3]   [z1 + 2z2 - 3z3]


*Dot product*
The dot product takes two equal dimension vectors and returns a single scalar value by summing the products of the vectors' corresponding component. The dot product operation is both commutative and distributive.
Formula:
a•b = ∑sum_i_from_1_to_n(ai * bi)

Basically: 
a•b = (a1 * b1) + (a2 * b2) +...

The dot product can also be used to find the magnitude of a vector and the angle between two vectors
- Magnitude of the vector is simply the square root of a vector's dot product with itself
||a|| = sqrt(a • a)

- Angle between two vectors: arccos of the dot product divided by the product of the vectors' magnitude
(The symbol is the capital theta θ but will use Ø for convenience)

Ø = arccos((a•b)/(||a|| * ||b||))

Ex find the angle between
	[3]		  [0]
a = [2]    b =[-3]
	[-3]	  [-6]

Ø = arccos((3*0 + 2*-3 + -3*-6) / (sqrt(3^2 + 2^2 + -3^2) * sqrt(0^2 + -3^2 + -6^2)))
Ø = 67.58°





## Matrices
A matrix is a quantity with m rows and n columns f data. For example we could combine multiple vectors into a matrix, where each column of the matrix is one of the vectors. Matrices are helpful becausethey allow us to perform operations on large amounts of data. 
Matrices are represented using square brackets enclosing rows and columns of data.
Matrices variables are conventially represent them with a capital letter
The shape of the matrix is said to be mxn, with m being rows and n being column.
	[a b c]
A = [d e f]
	[g h i]

The values within the matrix can be represented in the fashion Matrix m,n

ex
A1,2 = b
A2,3 = f


### Matrices operations
We can multiply entire matrices by a scalar value just by multiplying all elements for that value, as well as add or subtract matrices with equal shapes.
ex.
2[a1 b1] + 3[c1 d1] = [2a1+3c1 2b1+3d1]
 [a2 b2]	[c2 d2]   [2a2+3c2 2b2+3d2]


*Matrix multiplication*
Matrix multiplication works by computing the dot product between each row of the first matrix and each column of the second matrix. It is IMPORTANT that the sapes of the matrices must be such that the number of columns in A is equal to the number of rows in B. The matrix product will be the shape of rows * columns. This operation is not commutative but is associative.

ex.
		  [7 8]
[1 2 3] x [9 10]   = [((1*7)+(2*9)+(3*11)) ((1*8)+(2*10)+(3*12))] = [58 64]
[4 5 6]	  [11 12]	 [((4*7)+(5*9)+(6*11)) ((4*8)+(5*10)+(6*12))]	[139 154]




### Special Matrices

*Identity matrix*
The identity matrix is a square matrix of elements equal to 0, except for the elements along the diagonal that are equal to 1. Any matrix multiplied by the identity matrix, either on the left or right side, will be equal to itself.
[1 0 0]
[0 1 0]
[0 0 1]


*Transpose matrix*
The tranpose of a matrix is computed by swapping rows and columns of a matrix. The transpose operation is denoted by a superscript uppercase T (A^T)

[a b c]^T 	[a d g]
[d e f]   = [b e h]
[g h i] 	[e f i]



*Permutation matrix*
A permutation matrix is a square matrix that allows us to flip rows and columns of a separate matrix. Similar to the identity matrix it is made of 0 and 1. In order to flip _rows_ in the matrix A, we multiply our permutattion matrix (P) on the left (PA), while to flip columns, we multiply P on the right (AP)

	 [0 1 0] [a b c]   [d e f]
PA = [0 0 1] [d e f] = [g h i]
	 [1 0 0] [g h i]   [a b c]

	 [a b c] [0 1 0]   [c a b]
AP = [d e f] [0 0 1] = [f d e]
	 [g h i] [1 0 0]   [i g h]



## Linear systems in matrix form
Matrices are extremely useful to solve systems of linear equations. For example:

a1x + b1y + c1z = d1
a2x + b2y + c2z = d2
a3x + b3y + c3z = d3

This system of equations can be represented using vectors by combining coefficients of the same unknown variables, as well as the solutions into vectors. These vectors are then scalar multiplied by their known variable and summed

 [a1]	 [b1]	 [c1]	[d1]
x[a2] + y[b2] + z[c2] = [d2]
 [a3]	 [b3]	 [c3]	[d3]

Our goal is to represent this system in form Ax = b using matrices and vectors. We can combine vectors to create a matrix, which will do with the coefficient vectors to create matrix A. We can also convert the unknown variables into vector x, resulting in:

[a1 b1 c1] [x]	 [d1]
[a2 b2 c2] [y] = [d2]
[a3 b3 c3] [z]	 [d3]

We can also write Ax=B as [A|b]
Meaning that we can write our equation above as

[a1 b1 c1 | d1]
[a2 b2 c2 | d2]
[a3 b3 c3 | d3]

This is what is known as an _augmented matrix_, used to solve for linear systems.





## Gauss-Jordan elimination
Now that we have our system of linear equations in augmented matrix for we can solve the unknown variables with the Gauss-Jordan elimnination technique.

To do this, we want to put our augmented matrix into a _row echelon form_, where all elements below the diagonal are qual to 0:

[a1 b1 c1 | d1]    [1 b1' c1' | d1']
[a2 b2 c2 | d2] -> [0  1  c2' | d2']
[a3 b3 c3 | d3]    [0  0   1  | d3']

The values with apstrophes in the matrix, mean that they have been changed in the process of updating the matrix, once we do this, we can rewrite the orginal equation as

x + b1'y + c1'z = d1'
y + c2'z = d2'
z = d3'

Which then can be easily solved.

The process to reach this point is:
- Add or subtract rw 1 against all rows below in order to make all the elements in column 1 equal 0 under the diagonal.
- Once this is done, we can do the same with row 2 and all the rows below to make all elements below the diagonal in column 2 equal 0
- Once all elements below the diagonal are equal to 0, we can simply solve for the variable values, starting at the bottom of the matrix and working our way up. (This cannot be applied to all systems, an issue is getting 0z=4 which is impossible)


Example step by step:
- Put the system of equations into augmented matrix form
[ 1  1 -2 | 1]
[-1 -1  1 | 2]
[-2  2  1 | 0]


- Cancel out the first two entries in row 2 by adding the values of row 1 + row 2

			[ 1  1 -2 | 1]
!R2 + R1 -> [ 0  0 -1 | 3]
			[-2  2  1 | 0]


- Cancel out the first entry in row 3 by adding the values of row 1 to one-half of each value in row 3


				[ 1  1 -2 | 1]
				[ 0  0 -1 | 3]
!1/2 R3 + R1 -> [ 0  2 -3/2 | 1]


- Swap row 2 and row 3

[ 1  1 -2 | 1]
[ 0  2 -3/2 | 1]
[ 0  0 -1 | 3]

- Normalize the diagonals (make them all equal 1) by multiplying each value in row 2 by 1/2 and each value in row 3 by -1

		  [ 1  1 -2 | 1]
1/2 R2 -> [ 0  1 -3/4| 1/2]
 -1 R3 -> [ 0  0  1 | -3]


- Put augmented matrix into system of equations form
x + y + 2z = 1
y - 3/4z = 1/2
z = -3



## Inverse matrices
The inverse of a matrix, A^-1, is one where the following equation is true
`AA^-1 = A^-1A = I`
Meaning that the product of a matrix and its inverse, is equal to the identity matrix.

Inverse matrix is useful in many applications, for example solving equations with matrices:

xA = BC -> (multiply for inverse of A) -> xAA^-1 = BCA^-1 -> x = BCA^-1

!!!*Not all matrices have inverses*, those matrices are called _singular matrices_


*To find the inverse matrix, we can use the Gauss-Jordan elimination:*
Knowing that AA^-1 = I, we can create the augmented matrix [A|I], where we attempt to perform row operations such that [A|I] -> [I|A^-1]

One method to find the necessary row operations to make the conversion is to start by creating an upper tirangular matrix first, then work on converting the elements above the diagonal after starting from the bottom right side. If the martix is invertible, this should lead to a matrix with elements equal to 0, except along the diagonal.

Walkthrough example:

- Starting matrix

	[ 0  2  1]
A = [-1 -2  0]
	[-1  1  2]

- Put system of equations into [A|I] form

[ 0  2  1 | 1 0 0]
[-1 -2  0 | 0 1 0]
[-1  1  2 | 0 0 1]


- Swap row 1 and row 3
[-1  1  2 | 0 0 1]
[-1 -2  0 | 0 1 0]
[ 0  2  1 | 1 0 0]

- Subtract the values of row 1 from row 2 to cancel out the first entry in row 2
		[-1  1  2 | 0  0  1]
R2-R1 ->[ 0 -3 -2 | 0  1 -1]
		[ 0  2  1 | 1  0  0]


- Cancel out the first two entries of row 3 by adding 3/2 of each value of row 3 to row 2

			   [-1  1  2   |  0  0  1]
			   [ 0 -3 -2   |  0  1 -1]
3/2 R3 + R2 -> [ 0  0 -1/2 | 3/2 1 -1]



- Cancel out the second entry of row 1 by adding triple of each value of row 1 to row 2

3 R1 + R2 -> [-3  0  4   |  0  1  2]
			 [ 0 -3 -2   |  0  1 -1]
			 [ 0  0 -1/2 | 3/2 1 -1]


- Cancel out the third entry in row 2 by adding row 3 to -1/4 of each value of row 2

			   [-3  0  4  |  0  1  2]
-1/4R2 + R3 -> [ 0 3/4 0  | 3/2 3/4 -3/4]
			   [ 0  0 -1/2| 3/2  1  -1]


- Cancel out the third entry of row 1 by adding row 3 to 1/8 of each value of row 1

1/8R1 + R3 -> [-3/8 0  0  | 3/2 9/8 -3/4]
			  [ 0  3/4 0  | 3/2 3/4 -3/4]
			  [ 0   0 -1/2| 3/2  1   -1 ]


- Normalize the diagonals. Row 1 multiplied by -8/3 etc..

-8/3 R1 -> [1 0 0 | -4 -3  2]
 4/3 R2 -> [0 1 0 |  2  1 -1]
  -2 R3 -> [0 0 1 | -3 -2  2]


- FINALLY we obtain our inverse matrix:
[-4 -3  2]
[ 2  1 -1]
[-3 -2  2]








# Vectors and Matrices in Python (NumPy)
NumPy arrays are n-dimensional array data structures that can be used to represent both vectors and matrices in 1- and 2- dimensional arrays respectively.
They're initialised using np.array() including a Python list or nested list as an argument
```py
import numpy as np

v = np.array([1,2,3])
A = np.array([[1,2],[3,4]])
```

Matrices can also be created by combining existing vectors using the np.column_stack(). It uses the vectors as columns.
```py
v = np.array([-2,-2,-2,-2])
u = np.array([0,0,0,0])
w = np.array([3,3,3,3])
 
A = np.column_stack((v, u, w)) #Pass a tuple!!
print(A)

#print
#[[-2  0  3]
# [-2  0  3]
# [-2  0  3]
# [-2  0  3]]

```

*Indexing*

We can inspect the shape of the matrix using .shape and access individual elements using square brackets. Unlike python though we index in the same square bracket in the format [row,column] with 0 indexing
```py
A = np.array([[1,2],[3,4]])
print(A.shape)
print(A[0,1])

#Shape: (2,2)
#Indexing: 2

#Select dimensional subset of array using a column :
#Example we want the whole second column
print(A[:,1])

#[2 4] -> this is a column vector that outputs in the terminal horizontally

```


## Linear Algebra operation in numpy

*Addition and multiplication*
Addition and multiplication on numpy are doable using simple inbuilt python methods ( + * )
```py
#Multiplication
A = np.array([[1,2],[3,4]])
N = 4 * A

#Addition
B = np.array([[-4,-3],[-2,-1]])
A + B
```

*Vector dot products*
Vector dot products can be computed using np.dot()
```py
v = np.array([-1,-2,-3])
u = np.array([2,2,2])
np.dot(v,u)
```

*Matrix multiplication*
Matrix multiplication can be computed both using np.matmul() or with the shorthand @
```py
A = np.array([[1,2],[3,4]])
B = np.array([[-4,-3],[-2,-1]])
 
# one way to matrix multiply
np.matmul(A,B)
# another way to matrix multiply
A@B
```



### Special matrices
You can create special matrices using:

*Identity matrices*
```py
#4X4 identity matrix
identity = np.eye(4) #Where 4 is the n for the matrix n x n
```


*All-zero matrix or vector*
```py
#5-element vector of zeros
zero_vec = np.zeros((5))

#3x2 matrix of zeros
zero_mat = np.zeros((3,2))
```


*Transposed matrix*
Transposed matrix can be accessed with the attribute T
```py
A_transp = A.T
```


*Vector magnitude*
Calculating the norm or magnitude or length, requires accessing numpy sublibrary linalg
```py
v = np.array([2,-4,1])
v_norm = np.linalg.norm(v)
```


*Inverse of a square matrix*
If one exists
```py
A = np.array([[1,2],[3,4]])
print(np.linalg.inv(A))
```


*Solve for unknown variables in a system on linear equations in Ax=b form*
ex:
x + 4y -  z = -1
-x - 3y + 2z =  2
2x -  y - 2z = -2
```py
# each array in A is an equation from the above system of equations
A = np.array([[1,4,-1],[-1,-3,2],[2,-1,-2]])
# the solution to each equation
b = np.array([-1,2,-2])
# solve for x, y, and z
x,y,z = np.linalg.solve(A,b)

#prints 0, 0, 1, respective for x, y, z
```


























# Calculus

*Rate of Change*
Differential calculus is the field that studies rates of change by quantifying how variables relate to each other.

*Optimisation*
Mathematical optimisation problems involve looking for the best solution to a problem through statistical models that best fits our data.

*Infinitesimal analysis*
"To give a rough overview, however, the idea is that by ‘zooming in’ enough and considering how a variable changes over an infinitesimal time frame, we can learn how that variable is changing instantaneously, at a specific moment in time."



## Limits
Limits quantify what happens to a function as we approach a given point
(Hard to write the formula)
`limx->6 f(x) = L`
The limit as x goes to 6 of f(x) approaches some value L.
Basically we look at what the function looks like as it approaches 6 but never reaches it eg 5.9 5.99 5.999

Note: limits can approach both from the previous or the next value eg 5.9 - 6.1, if those limits do not approach the same value, the limit does not exist


Given that with limits we can approach smaller and smaller intervals, this gives us a way to calculate *derivatives* (in the graph are the lines parallel to a curve by touching only 1 point)

Derivatives are calculated with *derivative functions* which are extremely useful in giving info about the graph. 
- The derivative says nothing about the value of the original function, but only about how it's changing (ex we can calculate the speed of a car at a precise moment). 
- The derivatives give info on the direction of the change based on if f'(x) is < or > 0, if it is =0 it means the function has reached a minimum or maximum peak, which is important when looking trends.


*Power rule*
`d/dx x^n = n•x^n-1`


### Derivatives in python
Ex. We have an array representing the function
`f(x) = x^2 + 3`

```py
from math import pow
import numpy as np

# dx is the "step" between each x value
dx = 0.05
def f(x):
  # to calculate the y values of the function
  return pow(x, 2) + 3
# x values
f_array_x = [x for x in np.arange(0,4,dx)]
# y values
f_array_y = [f(x) for x in np.arange(0,4,dx)]

# Compute the derivative of f_array with gradients
f_array_deriv = np.gradient(f_array_y, dx)
```

# Important Concepts

- A limit is the value of a function approaches as we move to some x value.
- The limit definition of a derivative shows us how to measure the instantaneous rate of change of a function at a specific point.
- A derivative is the slope of a tangent line at a specific point. The derivative of a function f(x) is denoted as f’(x).
- The derivative of a function allows us to determine when a function is increasing, decreasing, or is at a minimum or maximum value.
































