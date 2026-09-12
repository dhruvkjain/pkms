[[crux_index]]

/ -> gives quotient
% -> gives remainder

- Longest subarray with sum k
	- prefix sum hashing (map)
- longest consecutive "sequence"
	- check it current element is starting of sequence (try finding current_element-1)
- lower bound of x
	- first element >= x in array
- upper bound of x
	- first element > x in array

- binary search on answers
	- find min / max type
		-  answer is a number, we can decide a range in which answer will lie from the question and then apply binary search on it (it is usually like after some value of answer all further elements are either not possible or possible)
	- find max(min) / min(max) type
		- max(min)  =  return high pointer
		- min(max)  =  return low pointer

- longest palindromic string
	- expand from middle of a substring
		-  odd length palindromic substring (i , j = middle = current element)
		-  even length palindromic substring (i = left middle = current element, j = right middle = current element+1)

- subsets without repetition
	- sort , then use recursion to generate subsets but skip duplicate elements
- combination sum (all subsequences that sum to k, each element can be taken any number of time) 
	- first pick , then not pick
- combination sum 2 (all subsequences that sum to k, a elements can be taken once only)
	- sort 
	- first pick , then not pick
	- use set to store all subsequences

- LRU cache
	- head and tail pointers for a doubly linked list 
	- hash map to store key_1  ->  pointer to doubly linked list node that has value_1
	- for get operation  =  use hash map to get the node then delete and insert the node (as it is recently used so bring it front)
	- for put operation
		- if already present update value
		- if new and number of node == capacity  =  delete lru node
		- add new node

- next greater element,

- BFS (queue only) , as queue is fifo it covers all neighbors / childrens before moving to next level
	- insert start element in queue
	- while (queue not empty)
		- q.front
		- pop it and check if its neighbors / childrens are visited or not
		- if not visited then add it them to queue
- DFS (stack or recursion) , as stack is lifo it goes down each level for a node then backtrack
	- insert start element in stack
	- while (stack not empty)
		- st.top
		- pop it and check if its neighbors / childrens are visited or not
		- if not visited then add it them to stack in reverse order (to maintain the order of results)

- Lowest Common Ancestor  =  write root to node traversal using dfs for both, then just compare

- Graphs
	- flood fill
	- word ladder
	- topo sort (DAG only) 
		- find in-degree for all nodes
		- enter all 0 in-degree nodes in queue
		- pop a node break all its connections (indegree-1 for all its neighbors)
		- now check if any neighbor has become node with 0 indegree, add them in queue
	- +ve only weighted DAG  =  Dijkstra
		- distance vector  =  all nodes to infinity, start / root node = 0
		- bfs using min-heap (priority queue) of (distance, node)
			- relaxation for each node
	- +ve and -ve weighted DAG  =  bellman ford 
		- distance vector  =  all nodes to infinity, start / root node = 0
		- relaxation (number of vertices - 1) times
		- if relaxation vth time cause any change in distance vector  =  -ve cycle
	- MST

- Disjoint Set / Union Find

DP
- Longest Common Subsequence
	- a pointers (i and j) for each string (s1 and s2)
	- if s1[i] == s2[j]
		- pick
		- 1 + lcs(i+1, j+1, s1, s2, dp)
	- else 
		- not pick
		- max (lcs(i+1, j, s1, s2, dp)  ,  lcs(i, j+1, s1, s2, dp))
- Longest Increasing Subsequence
	- we need previous value index which was taken and current idx
	- lis (idx, prev_idx)
		- not pick  ->  int len = lis (idx+1, prev_idx, dp) 
		- if (prev_idx == -1 || arr[idx] > arr[prev_idx])
		- pick  ->  len = max(len , 1+lis (idx+1, idx, dp))

