[[DSA_CP/index|DSA_CP/index]]

Striver sheet A2Z : 
https://takeuforward.org/strivers-a2z-dsa-course/strivers-a2z-dsa-course-sheet-2/

CP-31 sheet :
https://www.tle-eliminators.com/cp-sheet

Siddh recommendation :
https://youkn0wwho.academy/topic-list

Standard Questions :
[NeetCode](https://neetcode.io/)
[AlgoMap](https://algomap.io/)

CP-algorithms :
https://cp-algorithms.com/

CSES Problemset :
https://cses.fi/problemset/

Codeforces all useful blogs :
[I compiled a list of almost all useful blogs ever published on Codeforces [update: till 09.06.2021] - Codeforces](https://codeforces.com/blog/entry/91363)

Youtube channels : 
[Abdul Bari - YouTube](https://www.youtube.com/@abdul_bari)
[Algorithms Conquered - YouTube](https://www.youtube.com/@AlgorithmsConquered)
[CF Step - YouTube](https://www.youtube.com/@cfstepofficial)
[Errichto Algorithms - YouTube](https://www.youtube.com/@Errichto)
[Colin Galen - YouTube](https://www.youtube.com/@ColinGalen)
[Binary Box - YouTube](https://www.youtube.com/@binarybox3681)

Japanese ICPC contest website : 
[OI Wiki - OI Wiki (oi-wiki.org)](https://oi-wiki.org/)

[Kyopro Encyclopedia of Algorithms | Kyopro Encyclopedia of Algorithms (noshi91.github.io)](https://noshi91.github.io/algorithm-encyclopedia/)


> Time and Space Complexity :
> [LoveBabbar](https://www.youtube.com/watch?v=QovOdd80A4s)
> [Strivers A2Z DSA Course](https://www.youtube.com/watch?v=FPu9Uld7W-E

![[Pasted image 20240727142322.png]]

| Data Type              | Size (in bytes) | Range                           |
| ---------------------- | --------------- | ------------------------------- |
| short int              | 2               | -32,768 to 32,767               |
| unsigned short int     | 2               | 0 to 65,535                     |
| unsigned int           | 4               | 0 to 4,294,967,295              |
| ==int==                | 4               | -2,147,483,648 to 2,147,483,647 |
| long int               | 4               | -2,147,483,648 to 2,147,483,647 |
| unsigned long int      | 4               | 0 to 4,294,967,295              |
| ==long long int==      | 8               | -(2^63) to (2^63)-1             |
| unsigned long long int | 8               | 0 to 18,446,744,073,709,551,615 |
| signed char            | 1               | -128 to 127                     |
| unsigned char          | 1               | 0 to 255                        |
| ==float==              | 4               | -3.4×10^38 to 3.4×10^38         |
| ==double==             | 8               | -1.7×10^308 to1.7×10^308        |
| long double            | 12              | -1.1×10^4932 to1.1×10^4932      |
| wchar_t                | 2 or 4          | 1 wide character                |
## Important Takeaways :
- Never manipulate input data unless told.
- Noramlly servers do 10^8 operations per sec. So 2s ==> 2*(10^8) operations 


## Basics :
```cpp
#include <bits/stdc++.h>
using namespace std;

void doSomething(int &num){ // pass by reference
    num++;
    cout << "doSomething : " << num << endl;
}

void doNothing(int num){ // pass by value
    num++;
    cout << "doNothing : " << num << endl;
}

int main(){
	int x, y;
    cin >> x >> y ;
    cout << "value of x: " << x << " and y: " << y << endl;

    string s1, s2;
    cin >> s1 >> s2;
    cout << s1 << " " << s2 << endl;

    string str;
    getline(cin, str); // to get full line as cin don't take after a space
    cout << str << endl;
    cout << str[2] << endl; // str char can also be accessed like array
    cout << str.size() << endl; // find length of string
    str[2] = 'A';
    cout << str << endl;

    // array elements are stored in linear fashion (next to each other) im memory
    // 2D array ==> arr[row][col]

    int num = 7;
    doNothing(num);
    cout << num << endl;
    doSomething(num);
    cout << num << endl;
    
    // arrays are always passed as reference where address of first element (arr[0]) is passed
  
    return 0;
}
```

### sort function:
```cpp
#include <bits/stdc++.h>
#include <algorithm>
using namespace std;

int main()
{
    int arr[] = {1,7,2,12,43,4,100};
    int n = sizeof(arr)/sizeof(arr[0]);
    
    sort(arr, arr + n, [](int a, int b){
        return a>b;
    });
    
    for(int x:arr){
        cout << x << " ";
    }
    
    return 0;
}

// OUTPUT:
// 100 43 12 7 4 2 1
```

### priority queue:
```cpp
#include <bits/stdc++.h>
#include <algorithm>
using namespace std;

int main()
{
    priority_queue<int> pq;
    
    vector vec = {1,7,2,12,43,4,100};
    
    sort(vec.begin(), vec.end(), [](int a, int b){
        return a>b;
    });
    
    for(int x:vec){
        pq.push(x);
    }
    
    for(int x:vec){
        cout << pq.top() << " ";
        pq.pop();
    }
    
    return 0;
}
```


## Time Complexity :

> Time Complexity != Time taken by code to run
	 Rate at which time taken for code changes  == Time Complexity

>  Time Complexity has **Best Case, Average Case, Worst Case**
### Notations :
- Big O Notation               **Worst Case**
- Theta (θ) Notation          **Average Case**
- Omega (Ω) Notation       **Best Case**




## Space Complexity :

> ***Auxiliary Space*** : Space required to solve problem
> ***Input Space*** : Space required by inputs

```C++
int a; // Input Space
int b; // Input Space
cin >> a >> b;
int c = a + b; // Auxiliary Space
cout << c;
```




## Bit Manuplialtion :
[Bitwise operations for beginners - Codeforces](https://codeforces.com/blog/entry/73490)
https://youtu.be/LGrE0siZ-ZA?si=Ohjq6KG7pHqf8st0
### Even or Odd :
```cpp
if (x%2 == 0)    // even
if (x&1 == 0)    // even (takes O(1) time dbetter way)
```
as last bit of every even number is `0`

### Power of 2:
a number which is power of 2 `(4,8,16 etc...)` have only one bit as `1` other bits are `0`.
`2 = 10`
`4 = 100`
`8 = 1000`
`16 = 10000` etc...
if we do `x-1`
`1 = 01`
`3 = 011`
`7 = 0111`
`15 = 01111`
so if we do `x & x-1` then we get `0` which means x is an power of 2.
`10000 ==> 16`
	 `&`
`01111 ==> 15`
	 `=`
`00000 ==> 0`

> Note: but if `x = 0` then we will get power of 2.

CPP function to check if number is power of 2:
```cpp
bool powerof2(int x){
	return x && !( x&(x-1) )
}
```

### kth bit:
`1<<k = 2^k` obviously, just observe above example

> Toggle kth bit : `x ^ (1<<k)`
> Set kth bit (make kth bit `1`) : `x | (1<<k)`
> Unset kth bit (make kth bit `0`) : `x & ~(1<<k)`
 
Here `~` flip all the bits in a number.
Note : for `1<<k`, `~(1<<k) == (1<<k)-1`

### Multiply or Divide a number by 2^k
> `x * 2^k`  = `x<<k`
> `x / 2^k  = x>>k`

### x % 2^k
`x % 2^k === x & (2^k - 1) === x & (1<<k - 1)`

`x & (1<<k - 1)` here 
`1<< k` make `100000..` 
and `1<<k - 1` makes  `011111..`
now if we do AND the we get only last k bits of x which represents remainder.
### Swap 2 numbers using bit manipulation
`x = x^y` ==> `x = x^y , y = y`
`y = x^y` ==> `x = x^y , y = x^y^y = x`
`x = x^y` ==> `x = x^y^x = y , y = x`
### Extra Tricks
```cpp
if(x==a) x=b;
else if(x==b) x=a;
```
this same code can be written as
```cpp
x = a^b^x
```
as if x = a then a and x cancel out and only b is left, same for a. 

> Note :
> `A+B = (A^B) + 2(A&B)`
> `A+B = (A|B) + (A&B)`

to find number of set bits in a number (x) ==> `__buitin_popcount(x)` for int and `__builtin_popcountll(x)` for long long


## bitmask :

```C++
#include <iostream>
using namespace std;

void solve(){
    int a[20] = {1,2,3,4,5,6,7,8,9,10,11,12,13,15,16,17,18,19,20};
    int s = 135, n = 20;
    
    cout << (1<<n) << endl;
    
    for(int mask = 1; mask < (1<<n); mask++){
        long long sum_of_this_subset = 0;
        
        for(int i = 0; i < n; i++) {
    		if(mask & (1 << i)) {
    			sum_of_this_subset += a[i];
    		}
    	}
    	
    	if(sum_of_this_subset == s) {
    		cout<<"YES";
    		return;
    	}
    }
    cout << "NO";
	return;
}

int main()
{
    solve();
    return 0;
}
```
- In above code, `1 << n` == `2^n` generates all subsets for the array.
- So mask values iterates from 0 to 1048576 (2^n)  
- then we use mask as a map to guide us which element to take by `&` operator checking for each bit if we want to take that element represented by `i` bit.
> Time Complexity : O(2^n * n)


## bitsets :

```cpp
int num = 5
bitset<64>(num)  // 101
```
 like `__builtin_popcount()` bitsets has `num.count()` and other bit operation also work on bitsets.

## 2 pointers (squeeze)
let `l = start` and `r = end` be indices move `l` and `r` inside such that they squeeze and stop when `l<=r`. 
[2 Pointers Algorithm - DSA Course in Python Lecture 12](https://www.youtube.com/watch?v=syTs9_w-pwA)
```cpp
// Print all continuous sub-arrays.

ll l=0, r=n-1;
    
while(l<=r){
	for(ll i=l; i<=r; i++){
		cout << arr[i] << " ";
	}
	cout << endl;
	if(l==r && l<=n-1){
		l++;
		r=n;
	}
	r--;
};
```


# Question Patterns

1. Fast & Slow Pointer  
Detect cycles  
O(1) space  
Perfect for linked list traps  
  
2. Merge Intervals  
Sort + merge overlapping  
Must-know for calendar/booking apps  
  
3. Sliding Window  
Fixed/variable window  
O(n) time — used in 100+ array/string problems  
  
4. Matrix Traversal / Islands  
DFS + BFS in 2D grids  
Connected components, flood fill, etc.  
  
5. Two Pointers  
Start-end pointer strategy  
Avoids nested loops, speeds up logic  
  
6. Cyclic Sort  
Ideal for finding missing numbers  
O(n) time, O(1) space  
  
7. In-place Linked List Reversal  
Reverse without extra memory  
Key for real-world pointer skills  
  
8. BFS (Breadth First Search)  
Shortest path  
Level-order problems  
  
9. DFS (Depth First Search)  
Recursion/backtracking  
Tree/graph explorations  
  
10. Two Heaps  
Median from stream  
Min + Max heaps combo  
  
11. Subsets  
All combinations  
Powerset problems  
  
12. Modified Binary Search  
Search in rotated or weird arrays  
O(log n) magic  
  
13. Bitwise XOR  
Find missing or unique elements  
Constant space tricks  
  
14. Top K Elements  
Use heaps or quickselect  
Ranking & frequency problems  
  
15. K-way Merge  
Combine sorted arrays/lists  
Min-heap powered  
  
16. 0/1 Knapsack (DP)  
Pick or skip decisions  
Base of dynamic programming  
  
17. Unbounded Knapsack (DP)  
Use an item unlimited times  
Like coin change problems  
  
18. Topological Sort  
Task ordering  
Directed Acyclic Graphs  
  
19. Monotonic Stack  
Next greater/smaller element  
Clean up O(n²) to O(n)  
  
20. Backtracking  
Try → Recurse → Undo  
Subset, permutation, sudoku, N-Queens  
  
21. Greedy Algorithm  
Fast, local best strategy  
Works for interval, coin, and scheduling problems


# Merge Sort
```cpp
vector<int> sorter(vector<int> p1, vector<int> p2) {
	vector<int> res;
	
	int ptr1 = 0;
	int ptr2 = 0;
	while (ptr1<p1.size() && ptr2<p2.size()) {
		if (p1[ptr1] < p2[ptr2]) {
			res.push_back(p1[ptr1]);
			ptr1++;
		} else {
			res.push_back(p2[ptr2]);
			ptr2++;
		}
	}
	
	while (ptr1 < p1.size()) {
		res.push_back(p1[ptr1]);
		ptr1++;
	}
	
	while (ptr2 < p2.size()) {
		res.push_back(p2[ptr2]);
		ptr2++;
	}
	
	return res;
}

vector<int> mergesort(vector<int> nums, int start, int end) {
	vector<int> res;
	if (start == end) { 
		res.push_back(nums[start]);
		return res;
	}
	int half = start+((end-start)/2);
	vector<int> p1 = mergesort(nums, start, half);
	vector<int> p2 = mergesort(nums, half+1, end);
	return sorter(p1, p2);
}
```

**Questions**
- Count inversions
	```cpp
	int merge(vector<int> &arr, int low, int mid, int high) {
	    vector<int> temp; // temporary array
	    int left = low;      // starting index of left half of arr
	    int right = mid + 1;   // starting index of right half of arr
		
	    //Modification 1: cnt variable to count the pairs:
	    int cnt = 0;
		
	    //storing elements in the temporary array in a sorted manner//
		
	    while (left <= mid && right <= high) {
	        if (arr[left] <= arr[right]) {
	            temp.push_back(arr[left]);
	            left++;
	        }
	        else {
	            temp.push_back(arr[right]);
	            cnt += (mid - left + 1); //Modification 2
	            right++;
	        }
	    }
		
	    // if elements on the left half are still left //
		
	    while (left <= mid) {
	        temp.push_back(arr[left]);
	        left++;
	    }
		
	    //  if elements on the right half are still left //
	    while (right <= high) {
	        temp.push_back(arr[right]);
	        right++;
	    }
		
	    // transfering all elements from temporary to arr //
	    for (int i = low; i <= high; i++) {
	        arr[i] = temp[i - low];
	    }
		
	    return cnt; // Modification 3
	}
	
	int mergeSort(vector<int> &arr, int low, int high) {
	    int cnt = 0;
	    if (low >= high) return cnt;
	    int mid = (low + high) / 2 ;
	    cnt += mergeSort(arr, low, mid);  // left half
	    cnt += mergeSort(arr, mid + 1, high); // right half
	    cnt += merge(arr, low, mid, high);  // merging sorted halves
	    return cnt;
	}
	
	int numberOfInversions(vector<int>&a, int n) {
	    // Count the number of pairs:
	    return mergeSort(a, 0, n - 1);
	}
	
	int main()
	{
	    vector<int> a = {5, 4, 3, 2, 1};
	    int n = 5;
	    int cnt = numberOfInversions(a, n);
	    cout << "The number of inversions are: "
	         << cnt << endl;
	    return 0;
	}
	```


# XOR
- xor of two number is 0
- xor of 0 and number is number
- xor of a range (0 -> n)
	- if (n%4 == 0) return n
	- else if (n%4 == 1) return 1
	- else if (n%4 == 2) return n+1
	- else return 0


**Questions:**
- all numbers appear twice except on number => xor of all numbers
- find missing number in a range =>  xor of (xor of all numbers) and (xor of range) 
- Find repeating and missing number
	- intuition is when we take range and array missing number appears once and repeating appears thrice so xor of both will give  repeating ^ missing
	- then we can group all numbers according to the bit that is different in the xor we got in 2
	- then take xor in both groups
	```cpp
	vector<int> findMissingRepeatingNumbers(vector<int> a) {
	    int n = a.size(); // size of the array
		
	    int xr = 0;
		
	    //Step 1: Find XOR of all elements:
	    for (int i = 0; i < n; i++) {
	        xr = xr ^ a[i];
	        xr = xr ^ (i + 1);
	    }
		
	    //Step 2: Find the differentiating bit number:
	    int number = (xr & ~(xr - 1));
		
	    //Step 3: Group the numbers:
	    int zero = 0;
	    int one = 0;
	    for (int i = 0; i < n; i++) {
	        //part of 1 group:
	        if ((a[i] & number) != 0) {
	            one = one ^ a[i];
	        }
	        //part of 0 group:
	        else {
	            zero = zero ^ a[i];
	        }
	    }
		
	    for (int i = 1; i <= n; i++) {
	        //part of 1 group:
	        if ((i & number) != 0) {
	            one = one ^ i;
	        }
	        //part of 0 group:
	        else {
	            zero = zero ^ i;
	        }
	    }
		
	    // Last step: Identify the numbers:
	    int cnt = 0;
	    for (int i = 0; i < n; i++) {
	        if (a[i] == zero) cnt++;
	    }
		
	    if (cnt == 2) return {zero, one};
	    return {one, zero};
	}
	```


# Array

### Find All Duplicates in an Array (Negative marking)
because we are given that elements of array are in the range `[0 .. n]` we can uses elements as indexes
for every element in array mark it's absolute index -1 representing that element has been taken
```cpp
vector<int> findDuplicates(vector<int>& nums) {
	if(nums.size()==1) return {};
	// Negative marking
	// Whenever we meet the element we mark the value - 1 as negative
	vector<int> ans;
	for(int i = 0; i<nums.size(); i++) {
		if(nums[abs(nums[i])-1]>0)
			nums[abs(nums[i])-1] *=-1; // Mark it with negative
		else
			ans.push_back(abs(nums[i]));
	}
	return ans;
}
```

### Kadane's Algorithm
for each index we find max_sum till that index by greedily taking max of sum of current index and sum till now   
-  Initialize `max_current` and `max_global` to the first element.
-  For each element from index 1 onward:
    - Update `max_current` as the maximum of the current element or the sum of max_current and the current element.
    - Update `max_global` if `max_current` is greater.
- Return `max_global`.

**Questions:**
- find the subarray with maximum sum


### Dutch National flag Algorithm
we divide whole array in 3 parts and then start alloting according to the part's definition
- initalize `low = 0`, `mid = 0` and `high = arr.size()-1`
	- `[0 .. low-1]` -> 0's
	- `[low .. mid-1]` -> 1's
	- `[high+1 .. arr.size()-1]` -> 2's
	- so initially `mid` to `high` is our unsorted array i.e. `0` to `arr.size()-1`
- `while (mid <= high)`
	- if `arr[mid] == 0` 
		- => swap mid and low
		- => mid++ and low++ 
	- if `arr[mid] == 1`
		- => mid++
	- if `arr[mid] == 2`
		- => swap mid and high
		- => if (high != 0) high-- else break 
			- here we don't increase mid as after swap mid contains an unsorted number


**Questions:**
- Sort an array of 0’s 1’s and 2’s

### Stock Buy and Sell
- `maxPro = 0` and `minPrice = INT_MAX`
- for each day price `i` 
	- `minPrice = min(minPrice, arr[i])`
    -  `maxPro = max(maxPro, arr[i] - minPrice)`
- return `maxPro`

### Moore’s Voting Algorithm 
if an elements appears more than n/2 then while counting other elements will cancel the element but won't be able to reduce majority element to 0 as it appears more than n/2
for n/2
- `ele = arr[0]` and `cnt = 1`
-  for each index from `1` to `arr.size()-1`
	- `if (cnt == 0)` 
		- => `ele = arr[i]`
		- => `cnt = 1`
	- `else`
		- `if (arr[i] == ele)`
			- => `cnt++`
		- `else`
			- => `cnt--`
- return `ele` 

for n/3
- `ele1 = ele2 = INT_MIN` and `cnt1 = cnt2 = 0`
-  for each index from `0` to `arr.size()-1`
	- `if (cnt1 == 0 && arr[i] != ele2)` 
		- => `ele1 = arr[i]`
		- => `cnt1 = 1`
	-  `else if (cnt2 == 0 && arr[i] != ele1)`
		- => `ele2 = arr[i]`
		- => `cnt2 = 1` 
	- `else if (ele1 == arr[i])`
		- => `cnt1++`
	- `else if (ele1 == arr[i])`
		-  => `cnt2++`
	- `else` 
		- => `cnt1--`
		- => `cnt2--`
- return `ele` 

**Questions:**
- majority element (more than n/2 times)
- majority element (more than n/3 times)



### Next Permutation
for n length we have n! permutations
**Observations**
- for next permutation in sorted permutations array we need to have *longest prefix match*
- we need to play with the *slope of numbers in array*
	- we need to *find from back the first place where we see peak index*
		- peak index is where we see a number surrounded by two smaller numbers  
- For Example: `[2, 1, 5, 4, 3, 0, 0]`
- we can see that 5 is peak index so we need to change `1` that is miss placed to increase the number for next permutation
- now we can replace 1 with 5, 4 and 3 only as 0 is already smaller but we need number just greater than the current so we take 3 i.e. smallest greater element
- now for the rest keep it as smaller as possible
- `[2, 3, 0, 0, 1, 4, 5]`


- longer prefix match
	- for `i = [arr.size()-2 .. 0]`
		- `break_point = arr[i] < arr[i+1]`
		- break
- find element just greater than `break_point`
- fill the rest in sorted order
	- reverse the arr from `break_point` to `arr.size()-1`



### Longest Consecutive Subsequence
- create an `unordered_set` of array
- for each `ele` in `unordered_set`
	- check if it is the starting point i.e. if `ele-1` doesn't exit then
		- start finding the next elements and counting


### 3 Sum
```cpp
vector<vector<int>> triplet(int n, vector<int> &arr) {
    vector<vector<int>> ans;
    sort(arr.begin(), arr.end());
    for (int i = 0; i < n; i++) {
        //remove duplicates:
        if (i != 0 && arr[i] == arr[i - 1]) continue;
		
        //moving 2 pointers:
        int j = i + 1;
        int k = n - 1;
        while (j < k) {
            int sum = arr[i] + arr[j] + arr[k];
            if (sum < 0) {
                j++;
            }
            else if (sum > 0) {
                k--;
            }
            else {
                vector<int> temp = {arr[i], arr[j], arr[k]};
                ans.push_back(temp);
                j++;
                k--;
                //skip the duplicates:
                while (j < k && arr[j] == arr[j - 1]) j++;
                while (j < k && arr[k] == arr[k + 1]) k--;
            }
        }
    }
    return ans;
}
```

# Binary Search

### upper lower bound
```cpp
int lower_bound(vector<int> nums, int x) {
    int n = nums.size();
    int l = 0, r = n-1;
    int res = INT_MAX;
    
    while (l<=r) {
        int mid = (l+r) >> 1;
        if (nums[mid] > x) {
            res = nums[mid];
            r = mid-1;
        } else if (nums[mid] < x) {
            l = mid+1;
        } else {
            res = nums[mid];
            return res;
        }
    }
    
    return res;
}

int upper_bound(vector<int> nums, int x) {
    int n = nums.size();
    int l = 0, r = n-1;
    int res = INT_MIN;
    
    while (l<=r) {
        int mid = (l+r) >> 1;
        if (nums[mid] < x) {
            res = nums[mid];
            l = mid+1;
        } else if (nums[mid] > x) {
            r = mid-1;
        } else {
            res = nums[mid];
            return res;
        }
    }
    
    return res;
}
```

### Search in Rotated Sorted Array
if rotated then when applied binary search, it will divide array into two parts where 
- one part is sorted and other isn't (sorted part will have corners in right order, here corners are low - mid and mid - high pointer)
- check if element lies in the sorted part

### Find any peak element index in an array
use the similar concept of slope and peak as used in binary search
write cases when at start or end of array


## Binary Search on Answers find min or max
find the range of answers, the separating condition is always like after a certain element in range all further elements are either not possible or possible as answer

### Koko Eating Bananas
```cpp
long long timeReq(vector<int>& piles, int k){
	long long time = 0;
	for (int pile : piles) {
		time += (pile + cap - 1) / cap;
	}
	return time;
}
 
int minEatingSpeed(vector<int>& piles, int h) {
	int n = piles.size();
	if (n==1) return ceil((double)piles[0]/(double)h);
	
	int high = *max_element(piles.begin(), piles.end());
	int low = 0;
	
	while (low<high) {
		int mid = low + (high-low)/2;
		long long time_req = timeReq(piles, mid);
		if (time_req <= h) {
			high = mid;
		} else {
			low = mid+1;
		}
	}
	return low;
}
```

**Questions**
- Koko Eating Bananas
- Minimum days to make M bouquets:
- Find the Smallest Divisor Given a Threshold
- Capacity To Ship Packages Within D Days

### Kth Missing Positive Number
```cpp
int findKthPositive(vector<int>& arr, int k) {
	if (k<arr[0]) {
		return k;
	}
	int n = arr.size()-1;
	if (k>arr[n]) {
		return k+(n+1);
	}
	int low = 0;
	int high = n;
	
	while (low<=high) {
		int mid = (low+high)/2;
		int missing = arr[mid]-(mid+1); 
		if (missing<k) {
			low = mid+1;
		} else {
			high = mid-1;
		}
		// cout << low << " " << mid << " " << high << endl;
	}
	
	return k+high+1;
}
```

## Binary Search on Answers find min(max) or max(min)

- max(min) type ⇒ return high 
- min(max) type ⇒ return low

### Aggressive Cows
max(min) type
range of answers is `[1 .. max_distance_between_two_stalls]`
so apply binary search on this range and if for a given distance you could place cows then increase the distance otherwise decrease it
```cpp
bool canWePlaceCows (vector<int>& stalls, int dist, int cows) {
	int n = stalls.size()-1;
	int cntCows = 1;
	int last = stalls[0];
	
	for (int i=0; i<=n; i++) {
		if (stalls[i]-last >= dist) {
			cntCows++;
			last = stalls[i];
		}
		if (cntCows >= cows) return true;
	}
	
	return false;
}
 
int aggressiveCows(vector<int> &stalls, int k) {
	sort(stalls.begin(), stalls.end());
	
	int n = stalls.size() - 1;
	int low = 1;
	int high = stalls[n] - stalls[0];
	
	while (low<=high) {
		int mid = (low+high)/2;
		if (canWePlaceCows(stalls, mid, k)) {
			low = mid+1;
		} else {
			high = mid-1;
		}
	}
	return high;
}
```

### Allocate Minimum Number of Pages
min(max) type
maximum number of pages assigned to a student is minimum
so range of answers is `[max_pages .. sum_of_all_pages]`
so apply binary search on this range and if for a given capacity of allocation of pages you could allot all books then increase the capacity otherwise decrease it
```cpp
bool canAllotPage(vector<int>& pageNumbers, int m, int capacity) {
	int n = pageNumbers.size()-1;
	int alloted = 0;
	int currSum = 0;
	for (int i=0; i<=n; i++) {
		if (currSum+pageNumbers[i] > capacity) {
			alloted++;
			currSum = pageNumbers[i];
		} else {
			currSum += pageNumbers[i];
		}
	}
	cout << capacity << " " << currSum << " " << alloted << endl;
	if (alloted >= m) {
		return true;
	} else {
		return false;
	}
}
 
int allocatePages(vector<int>& pageNumbers, int m) {
	int n = pageNumbers.size() - 1;
	if (m > (n+1)) return -1;
	int low = *max_element(pageNumbers.begin(), pageNumbers.end());
	int high = accumulate(pageNumbers.begin(), pageNumbers.end(), 0);
	 
	while (low<=high) {
		int mid = (low+high)/2;
		cout << low << "-" << mid << "-" << high << endl;
		if (canAllotPage(pageNumbers, m, mid)) {
			low = mid+1;
		} else {
			high = mid-1;
		}
	}
	return low;
}
```

**Questions**
min(max) type
- Allocate Minimum Number of Pages
- Split Array - Largest Sum
- Painter’s Partition Problem
- Minimize Maximum Distance between Gas Stations


# Strings

### Longest Palindromic Substring
treat each character (single for odd length and two for even length) as a potential center of a palindrome and expand outward to find the longest palindrome



# Linked List

### Middle element in a Linked List
- two pointer fast and slow such that fast moves 2 * slows 
- so in O(N/2) we can find middle element.

this method can be used to find nth element from array too by moving fast += n and slow += 1

### Reverse a linked list (iterative)
```cpp
ListNode* reverseList(ListNode* head) {
	ListNode* prev = nullptr;
	ListNode* next = head;
	ListNode* curr = head;
	
	while (next!=nullptr) {
		next = curr->next;
		curr->next = prev;
		prev = curr;
		curr = next;
	}
	
	return prev;
}
```

### Detect Loop in Linked List using 2 pointer (fast and slow)
- inside loop, slow move by 1 and fast by 2 
- so each iteration causes distance between them to reduce by 1
- eventually slow catch up and is at same node as fast which signifies that LL has loop

### Find Starting point of Loop in Linked List (fast and slow)
- If distance from starting of Linked List to starting of Loop is `d`, 
- then the collision point of fast and slow is always mid point of loop, 
- which is `d` so: 
- head ⇐ d ⇒ loop starting ⇐ d ⇒ collision point ⇐ d ⇒ loop starting

# Recursion

### Subsets II
array has duplicate elements but the power set must not contain duplicate subsets
- sort the array
- start a recursion where we carry a data structure
- add unique elements from array to data structures and call recursion again
- note that here we each recursion level have data structure of same length

```cpp
void findSubsets(int pos, vector<int>& nums, vector<vector<int>>& ans, vector<int>& ds){
	ans.push_back(ds);
	for (int i=pos; i<nums.size(); i++){
		if(i!=pos && nums[i] == nums[i-1]) continue;
		ds.push_back(nums[i]);
		findSubsets(i+1, nums, ans, ds);
		ds.pop_back();
	}
}
 
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
	vector<vector<int>> ans;
	vector<int> ds;
	
	sort(nums.begin(), nums.end());
	findSubsets(0, nums, ans, ds);
	
	return ans;
}
```

### Combination Sum
all subsequences that sum up to target and each element can be selected multiple times
- start a recursion where we carry a data structure
- we can either pick element at pos or not pick

```cpp
void find_combinations (int pos, int target, vector<int>& candidates, vector<vector<int>>& ans, vector<int>& ds) {
	if (pos == candidates.size()) {
		if (target == 0){
			ans.push_back(ds);
		}
		return;
	}
	
	if (candidates[pos] <= target) {
		ds.push_back(candidates[pos]);
		// taking the same element again
		find_combinations(pos, target - candidates[pos], candidates, ans, ds);
		// after backtracking if this branch final ds does not sum up to target we pop back the element
		ds.pop_back();
	}
	
	// taking the next element
	find_combinations(pos+1, target, candidates, ans, ds);
}
 
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
	vector<vector<int>> ans;
	vector<int> ds;
	
	find_combinations(0, target, candidates, ans, ds);
	return ans;
}
```

### Combination Sum II
all subsequences that sum up to target and each element can be selected only once

```cpp
void find_unique_combinations (int pos, int target, vector<int>& candidates, set<vector<int>>& ans, vector<int>& ds) {
	if (pos == candidates.size()) {
		if (target == 0){
			if (ans.find(ds) == ans.end()) {
				ans.insert(ds);
			}
		}
		return;
	}
	
	if (candidates[pos] <= target) {
		ds.push_back(candidates[pos]);
		find_unique_combinations(pos+1, target - candidates[pos], candidates, ans, ds);
		ds.pop_back();
	}
	
    find_unique_combinations(pos+1, target, candidates, ans, ds);
}
 
vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
	set<vector<int>> ans;
	vector<int> ds;
	vector<vector<int>> res;
	sort(candidates.begin(), candidates.end());
	find_unique_combinations(0, target, candidates, ans, ds);
	for (auto i : ans) {
		res.push_back(i);
	}
	
	return res;
}
```








