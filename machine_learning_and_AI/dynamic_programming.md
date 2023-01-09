# Dynamic programming

## Definitions

Memoization: storing of recursive subproblems' results so not to re-do it

Tabulation:




# Fibonacci sequence

(Generates the next number of the sequence by summing the previous two, the two starting ones are 1)

Example in js
```js

const fib = (n) =>{
	if(n<=2) return 1

	return fib(n-1) + fib(n-2)
}


```
O(2^n) time
O(n) space

This Fibonacci function is very slow given by the number of recursive calls

Give the example fib(7) (The numbers represent n with which the function is called) (fib(2/1) returns 1)

`                           (7)
                  ----------/ \--------
                (6)                   (5)
		  ------/ \------          ---/ \---
		(5)             (4)      (4)      (3)
	  --/ \--	        / \      / \      / \
    (4)  	(3) 	  (3) (2)  (3) (2)  (2) (1)
	/ \		/ \ 	  / \       / \
  (3) (2) (2) (1)   (2) (1)   (2) (1)
  / \
(2) (1)`

*This will have a O(n^2) time complexity*
_Only has O(n) space complexity as it refers to the height of the tree_



# Optimising solution with memoisation
We store the calculated values and reuse them

```js
//store duplicate subproblems
// in object, keys = arg to fn, value = return value
const fib = (n, memo = {}) => {
	//Check for existance of case in memo, if yes return it
	if (n in memo) return memo[n];
	if (n <= 2) return 1;
	//save value in memo object and pass it in the new calls 
	memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
	return memo[n]
}
```
O(n) time and space complexity



# Grid problem with memoization
Problem:
You're a traveler on a 2D grid. You begin in the top left corner and the goal is to travel to the bottom right. Can only move down or right. 
How many ways can you travel to the goal on a grid with dimensions m•n?

Ex in 2x3 grid -> 3 ways

[ ] [ ] [ ]
[ ] [ ] [ ]

1: right, right down
2: r, d, r
3: d, r, r


```js
const gridTraveler = (m, n) => {
	//if the grid is a 1x1 return 1
	// we return 1 because accounts for the 'branching' having reached a single square so only by moving down and right has to be the bottom right
	if(m === 1 && n === 1) return 1;
	// if one dimension is 0 return 0
	// this means the 'branching' has reached a dead end and doesn't account for a possible path
	if(m === 0 || n === 0) return 0;
	// return move to right (eg m-1) and moving down eg n-1
	return gridTraveler(m - 1, n) + gridTraveler(m, n-1)
}

````
O(2^n+m) time
O(n+m) space




## Improving runtime with memoization

```js
const gridTraveler = (m, n, memo={}) => {
	//Check if the arguments are in the memo
	// the key is going to be a string m,n
	const key = m+','+n;
	if (key in memo) return memo[key];

	if(m === 1 && n === 1) return 1;
	if(m === 0 || n === 0) return 0;
	
	//Save in object and send object when calling 
	memo[key] =  gridTraveler(m - 1, n, memo) + gridTraveler(m, n-1, memo)
	return memo[key]
}

```
O(n+m) for time and space complexity




# Memoisation strategy
First just have a solution
- Visualise the problem as a tree
- Implement the tree using recursion
- Has to work

Then make it efficient
- Add memo object
- Add a base case to return memo values
- Store return values into the memo



# Another dynamic problem
Write a function 'canSum(targetSum, numbers)' that takes in a targetSum and an array of numbers as arguments.
The function should return a boolean indicating whether or not it is possible to generate the targetSum using numbers from the array.

You may use an element of the array as many times as needed
You may assume that all input numbers are nonnegative

ex
canSum(7, [5,3,4,7]) -> true bc 3+4 = 7

The function has to 'branch' using each of the numbers in the array and subtracting it from the target sum. For each branch, it iterates by subtracting the all the numbers in the array from the obtained result once again and so on until it gets 0. Branches that reach negative numbers or are lower than the lowest number in the array are discarded. 

```js

const canSum = (targetSum, numbers) => {
	//base in case the target sum is 0 meaning we reached a possible sum
	if (targetSum === 0) return true

	//if the remainder is negative, it means it has gone too far and has to stop that branching
	if (targetSum < 0) return false

	//iterate through the number array 
	for (let num of numbers){
		//get the new remainder
		const remainder = targetSum - num
		//call recursively with the same numbers (as we can use any any amount of times, with the remainder)
		//If it is possible to generate a sum, say it's true
		if(canSum(remainder, numbers) === true){
			return true
		}
	}

	//base case, if it hadn't returned true, it means it's false
	return false
}

console.log(canSum(7, [2, 3]))//true
console.log(canSum(7, [2, 4]))//false


```
m = target sum
n = array length

Time complexity:
O(n^m)

Space complexity:
O(m)




### Improve with memoization

```js

const canSum = (targetSum, numbers, memo={}) => {
	//check if the targetSum is in the memo, if it is return the memoized case (no need to worry about numbers as they never change)
	if (targetSum in memo) return memo[targetSum]

	if (targetSum === 0) return true

	if (targetSum < 0) return false

	for (let num of numbers){
		const remainder = targetSum - num
		//pass the memo
		if(canSum(remainder, numbers, memo) === true){
			//if true, add case in memoization
			memo[targetSum] = true
			return true
		}
	}

	return false
}

```
Time complexity:
O(m•n)

Space complexity:
O(m)





# New problem: How sum
Write a function 'howSum(targetSum, numbers)' that takes in a target number and an array of arguments.
The function should return an array containing any combinarton of elements that add up exactly to the targetSum. If there is no combination, then return null.

Ex.
howSum(8, [5,3,4]) -> [5, 3]
howSum(7, [4,2]) -> null


```js
//Base case: if the targeSum is 0, return an empty array

const howSum = (targetSum, numbers) => {
	if (targetSum === 0) return []
	// If we went negative
	if (targetSum < 0) return null

	//Iterate through the numbers and get the remainder
	for (let num of numbers){
		const remainder = targetSum - num

		//Get iterative call result
		const remainderResult = howSum(remainder, numbers)

		//if doens't return null (means it is possible to generate a remainder)
		if (remainderResult !== null){
			//we return the remainder result, with appended the number in the iteration that made a non null call return
			return [...remainderResult, num]
		}

	}

	//return null if nothing was generated
	return null
}

```
m = target sum
n = array length

Time complexity:
O(n^m•m)

Space complexity:
O(m)



### Improve with memoization
```js

const howSum = (targetSum, numbers, memo={}) => {
	//check the memo
	if (targetSum in memo) return memo[targetSum]

	if (targetSum === 0) return []
	if (targetSum < 0) return null

	for (let num of numbers){
		const remainder = targetSum - num
		//pass memo too
		const remainderResult = howSum(remainder, numbers, memo)

		if (remainderResult !== null){ 
			//save value in memo
			memo[targetSum] = [...remainderResult, num]
			return memo[targetSum]
		}



	}
	//base case
	memo[targetSum] =null
	return null
}

```
Time complexity:
O(n•m^2)

Space complexity:
O(m^2)






# Best sum problem
Write a function 'bestSum(targetSum, numbers)', exactly the same as the one before but has to return the shortest combination of numbers.

Overall logic:
Find all ways to generate a number, when we find one shorter than the current, we replace it


```js

const bestSum = (targetSum, numbers) =>{
	//base cases
	if (targetSum === 0) return []
	if (targetSum < 0) return null


	//Shortest combination variable
	let shortestCombination = null


	//iterate through numbers
	for (let num of numbers){
		//get the remainder and call the function iteratively
		const remainder = targetSum - num
		const remainderCombination = bestSum(remainder, numbers)
		// if the call doesn't return null
		if (remainderCombination !== null){
			const combination = [...remainderCombination, num]

			//If the combination is shorter than the current 'shortest', update it
			// check if shortest combination is not null
			if (shortestCombination === null || combination.length < shortestCombination.length){
				shortestCombination = combination
			}


		}
	}

	return shortestCombination
}

```
m = target sum
n = array length

Time complexity:
O(n^m•m)

Space complexity:
O(m)




### Improved with memoization

```js

const bestSum = (targetSum, numbers) =>{
	//memo checking logic
	if (targetSum in memo) return memo[targetSum]

	if (targetSum === 0) return []
	if (targetSum < 0) return null

	let shortestCombination = null


	for (let num of numbers){
		const remainder = targetSum - num
		const remainderCombination = bestSum(remainder, numbers, memo)
		if (remainderCombination !== null){
			const combination = [...remainderCombination, num]

			if (shortestCombination === null || combination.length < shortestCombination.length){
				shortestCombination = combination
			}


		}
	}

	memo[targetSum] = shortestCombination

	return shortestCombination
}

```
Time complexity:
O(n•m^2)

Space complexity:
O(m^2)



---

Timestamp 2:12:35


















