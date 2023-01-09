# Dijkstra algorithm

Needs 2 lists, one for visited and one for unvisited. Lists are composed of 
- Vertex
- Shortest distance from A (set to infinity)
- Previous vertex



Works as, 
- from the starting vertex we check all connected nodes and their distances.
	- If their calculated distance is less than the one stored before we replace it (for first iterations we always do)
		- When replacing remember to add the vertex in the corresponding column

- Now we have to move A (starting point) to visited and we won't access it anymore


*Move to next vertex*

- Now we go from one of the previously visited vetex to the UNVISITED ones (the distance has to be calculated starting from the starting vertex A)

- If the calculated distance of the vertex is less than the previously stored remember to update it




Algorithm pseudocode for finding all paths
(Greedy algorithm)

- Let distance of start vertex (A) from start vertex (A) = 0
- Let distance of all other vertices from start = ∞

While True:
- Visit the unvisited vertex with the smallest known distance from the start vertex
- For the current vertex, examine its unvisited neighbours
- For the current vertex, calculate distance of each neighbout from start vertex
- If the calculated distance of a vertex is less than the known distance, update the shortest distance
- Update the previous vertex for each of the updated distances
- Add the current vertex to the list of visited vertices

! If all vertices were visited break





# Algorithm for minimum distance to a point

- Let distance of start vertex (A) from start vertex (A) = 0
- Let distance of all other vertices from start = ∞

WHILE there are unvisited vertices:
- Visit unvisited vertex with smallest known distance from start vertex 
- FOR Each unvisited neighbour of the current vertex
	+ Calculate the distance from start vertex
	+ IF the calculated distance of this vertex is less than the known distance
		* Update shortest distance to this vertex
		* Update the previous vertex with the current vertex
	+ END IF
- NEXT unvisited neighbour
- Add the current vertex to the list of visited vertices



--------

Ongoing

