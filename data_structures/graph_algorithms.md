# Graphs


# Definitions

Graphs: collection of nodes and edges
- Nodes/Vertex: visualisable as data points
- Edges: connection between nodes

Graphs can be either directed or undirected. Directed have directional nodes, undirected are free movement

Neighbour nodes are nodes immediately accessible from the starting point (respecting directionality if any enforced)

Graph cycle is a loop inside a directional graph which cannot be exitex

Undirected graph => graph represented by connections of two nodes (non directional) 



### Graph informations are represented as *adjacency lists*

- Graph

(a) -> (c)
 |		|
 v 		v
(b) <- (e)
 |
 v
(d) <- (f)

- Adjacency list -> (hashmap data structure eg object, dictionary, unordered map)
{
	a: [b, c],
	b: [d],
	c: [e],
	d: [], -> even if empty
	e: [b],
	f: [d]
} -> These are all the accessible values





# Depth/Breath first trasversal algorithm

Depth first trasversal algorithm follow the path till the end before choosing adjacent path (Top to top - Stack) (In the example graph goes A-B-D-C-E)
Breath first instead do all adjacent nodes before moving to deeper levels (Top to bottom - Queue) (In the example graph goes A-B-C-D-E)


Pseudocode for depth first
- Put A in the stack
- Access A and get its neighbours, push neighbours (B-C) to stack
- Stack has data so pick node from top
- Top is B -> neighbour is D -> D goes on top of the stack (get priority on C)
- Top is D -> D has no neighbours so you can pick C etc...


Pseudocode for breath first
Same as before but instead of adding to stack you add to Queue and you pick from bottom instead of top

CODES iterate until stack/queue are empty


CODE IN JAVASCRIPT FOR DEPTH FIRST

```js
const depthFirstPrint = (graph, source) => { //Iterative
	//initialise stack
	const stack = [source]

	while (stack.length > 0){
		//Get last element of array
		current = stack.pop()
		//Print node
		console.log(current)
		//get neigbours
		for (let neighbor of graph[current]){
			stack.push(neighbor)
		}
	}

}

//Same problem but recursively
const depthFirstPrintRecursive = (graph, source) =>{ //Recursive
	console.log(source)
	for (let neighbor of graph[source]){
		depthFirstPrintRecursive(graph, neighbor)
	}
}


const graph = {
	a: ['a', 'c'],
	b: ['d'],
	c: ['e'],
	d: ['f'],
	e: [],
	f: []
}




```


CODE IN JS FOR BREATH FIRST (only possible iteratively)
```js

const breathFirstPrint = (graph, source) =>{
	const queue = [source]
	while (queue.length > 0) {
		const current = queue.shift()
		console.log(current)
		for (let neighbor of graph[current]){
			queue.push(neighbor)
		}
	}
}



```





# HAS PATH PROBLEM

Determine if there is a path between source and destination

Approach -> depth/breath first doesn't matter, for each iteration check if current == destination

*complexity*

n = # nodes
e = # edges (n^2)
Time: 0(e)
Space: 0(n)


CODE IN JS
```js

const hasPath = (graph, src, dst) =>{
	if (src === dst) return true

	for (let neighbor of graph[source]){
		if (hasPath(graph, neighbor, dst) === true){
			return false
		} 
	}

	return false
}

```

Non iterative (breath first)
```js

const hasPath = (graph, src, dst) =>{
	const queue = [src]

	while (queue.length > 0){
		const current = queue.shift()
		if(current === dst) return true

		for (let neighbor of graph[current]){
			queue.push(neighbor)
		}
	}

	return false
}


```


------

# Undirected path algorithm

You usually get an edges list ->
edges: [
	[i, j],
	[k, i]...
]


Then you can transform it in an adjacency list 

graph: {
	i: [j, k],
	j: [i, k],
	k: [i, m, l, j],
	m: [k],
	l: [k],
	o: [n],
	n: [o]
}

*Visual*

(i) -- (j) -> j and k are linked creating a cycle
 | /-/  |
(k) -- (l)
 |
(m)

(o) -- (n) -> trivial cycle as can be a loop



*To avoid get stucked in loops you need to implement a visited list*

This example is the same as before, says if the node dst is accessible from src, it only guards for loop on top of it


```js

//helper function to transform edges list to undirected path
const buildGraph = (edges) =>{
	const graph = {}

	for (let edge of edges){
		const [a, b] = edge
		//if a or b are not initialised in the graph initialise them
		if(!(a in graph)) graph[a] = []
		if(!(b in graph)) graph[b] = []
		// push a to b and b to a
		graph[a].push(b)
		graph[b].push(a)
	}
	return graph
}

//Depth first recursive

const undirectedPath = (edges, nodeA, nodeB, new Set() ) =>{ // -> sets are fasted than array to check+add
	const graph = buildGraph(edges)
	return hasPath(graph, nodeA, nodeB)
}

const hasPath = (graph, src, dst, visited) =>{
	if(src === dst) return true
	//check if node was visited
	if (visited.has(src)) return false

	//add node to visited
	visited.add(src)

	for (let neighbor of graph[src]){
		if(hasPath(graph, neighbor, dst, visited) === true){
			return true
		}
	}

	return false
}

```






------


# Connected components count

Algorithm to count the number of 'groups' of nodes (in the example before was 2)

Time complexity: O(e)
Space complexity: O(n)


This case's graph

graph: {
	3: [],
	4: [6],
	6: [4, 5, 7, 8],
	8: [6], 
	7: [6],
	5: [6],
	1: [2],
	2: [1]
}

Representation:

(1) -- (2)
       (4)
     	|
(5) -- (6) -- (8)
		|
	   (7)
(3)

Components :3 -> (1,2) - (3) - (4,5,6,7,8)


Pseudocode:
- Iterating through all nodes!
- Start at node 1 doing a traversal
	+ Mark 1 as visited
	+ Reach 2 and mark it as visited
	+ Cannot reach any other nodes from there, increment components count
- Fallback to next node
	+ Node 2 was already visited so no traversal needed, skip
- Fallback to next node
	+ Mark 3 as visited
	+ Cannot reach any other nodes from there, increment components count
- Fallback to next node
	+ ...


CODE IN JS
```js

const explore = (graph, current, visited) =>{
	if (visited.has(current)) return false

	visited.add(current)

	for (let neighbor of graph[current]){
		explore(graph, neighbor)
	}

	return true
}


const connectedComponentsCount = (graph) =>{
	const visited = new Set()

	//Begin traversal at each potential node
	for (let node in graph){
		if(explore(graph, node, visited) === true){
			count += 1
		}
	}
	return count
}

```




----------------

# Largest component algorithm

Time complexity: O(e)
Space complexity: O(n)

Graph: 
graph:{
	0: [8,1,5],
	1: [0],
	5: [0, 8],
	8: [0, 5],
	2: [3, 4],
	3: [2, 4],
	4: [3, 2]
}

Representation:

    (5)
     | \
(1)-(0)-(8)

(4)-(2)
  \ /
  (3)

Largest component: 4

Pseudocode:
- Start iterating from first node
	+ Node 0
		* Mark it as visited + traverse all adjacent nodes
		* When not possible traversing anymore count traversed components and store variable
- Same as before
	+ If current count is larger than stored replace it


CODE IN JS

```js

const exploreSize = (graph, node, visited) =>   {
	if(visited.has(node)) return 0
	visited.add(node)

	let size = 1
	for (let neighbor of graph[node]){
		size += exploreSize(graph, neighbor, visited)
	}
	
	return size
}


const largestComponent = (graph) =>{
	const visited = new Set()
	let longest = 0
	for (let node in graph){
		const size = exploreSize(graph, node, visited)
		if(size > longest) longest = size
	}
	return longest
}



```


----------


# Shortest path algorithm

Complexity:
Time: O(n)
Space: O(e)

Breath first approach is much better


Pseudocode:
- The queue is populated with the starting point
- The start node becomes current
- Loop
	+ Check neighbours of current
		* Add node to queue together with distance ex (x, 1)
		* Do this for all neighbours
	+ Next element in queue

	+ When starting from next node remember to add distance and not replace it

	+ Once reached the destination node return result


CODE IN JS

```js

const buildGraph = (edges) =>{
	const graph = {}

	for (let edge of edges){
		const [a, b] = edge
		if (!(a in graph)) graph[a] = []
		if (!(b in graph)) graph[b] = []

		graph[a].push(b)
		graph[b].push(a)
	}
}



const shortestPath = (edges, nodeA, nodeB) =>{
	const graph = buildGraph(edges)

	//initialise visited
	const visited = new Set([nodeA])

	//initialised queue
	const queue = [ [nodeA, 0]]


	while(queue.length > 0){
		const [node, distance] = queue.shift()

		//if node is nodeB return distance
		if (node === nodeB) return distance

		for (let neighbor of graph[node]){
			if (!visited.has(neighbor)){
				visited.add(neighbor)
				queue.push([neighbor, distance+1])
			}
		}
	}

	//in case there is no path
	return -1


}

```

-----------

# Island count problem

We want to count the number of islands in the array 
Island: horizontal or vertical interconnected land

#where water is w and land is l
grid_graph = 
[
	'w', 'l', 'w', 'w', 'l', 'w',
	'l', 'l', 'w', 'w', 'l', 'w',
	'w', 'l', 'w', 'w', 'w', 'w',
	'w', 'w', 'w', 'l', 'l', 'w',
	'w', 'l', 'w', 'l', 'l', 'w',
	'w', 'w', 'w', 'w', 'w', 'w',
]
visualised:
•x••x•
xx••x•
•x••••
•••xx•
•x•xx•
••••••

Approach: 
- Declare
	+ Island counter
	+ Visited tuples
- Iterate through graph until land is found
	+ Mark as visited when interacting with a tuple
	+ If tuple is visited continue
- When find a piece of land use a depth first approach until no more moves are left in the stack
	+ Mark +1 island and move to next iteration


Notation of the code

r = #rows
c = #cols

Time: O(rc)
Space: O(rc)



JavaScript code

```js

const explore = (grid, r, c)=>{
	//Checking if we're inside the grid
	const rowInbounds = 0 <= r && r < grid.length
	const colBounds = 0 <= c && c < grid.length //TO CHANGE
	if(!rowInbounds || !colBounds) return false

	//if water
	if(grid[r][c] === 'w') return false


	//doing this bc set won't accept arrays
	const pos = r+','+c
	//if position was visited
	if(visited.has(pos)) return false
	visited.add(pos)

	//transversal now
	explore(grid, r-1, c, visited)
	explore(grid, r+1, c, visited)
	explore(grid, r, c-1, visited)
	explore(grid, r, c+1, visited)

	return true
}


const islandCount = (grid) =>{
	const count = 0
	const visited = new Set()
	for(let r=0; r < grid.length; r++){
		for(let c=0; c < grid(r).length; c++){
			if(explore(grid, r, c, visited) === true){
				count ++
			}
		}
	}
}


```



# Minimum island problem

Define minimum island size

Approach:
- Nested code + transversal depth first approach
- Loop through the graph and retrieve the n of blocks that compose the island
	+ Remember to add the visited places
	+ Compare to current minsize, if smaller replace


```js
 

const exploreSize = (grid, r, c, visited) =>{
	const rowInbounds = 0 <= r && r < grid.length
	const colBounds = 0<= c && c<grid[0].length
	if(!rowInbounds || !colBounds) return 0

	if(grid[r][c] === 'w') return 0

	const pos = r+','+c
	if (visited.has(pos)) return 0
	visited.add(pos)
	
	let size = 1
	size += exploreSize(grid, r-1, c, visited)
	size += exploreSize(grid, r+1, c, visited)
	size += exploreSize(grid, r, c-1, visited)
	size += exploreSize(grid, r, c+1, visited)

	return size
} 


 const minimumIsland = (grid) =>{
 	let minSize = Infinity
 	for (let r=0; r<grid.length; r+=1){
 		for (let c=0; c <grid[0].length; c+=1){
 			const size = exploreSize(grid, r, c, visited)
 			if (size < minSize && size > 0){
 				minSize = minSize
 			}

 		}
 	}

 	return minSize

 }




```









