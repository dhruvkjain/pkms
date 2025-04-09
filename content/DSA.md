[[index]]

Striver sheet A2Z : 
https://takeuforward.org/strivers-a2z-dsa-course/strivers-a2z-dsa-course-sheet-2/

Siddh recommendation:
https://youkn0wwho.academy/topic-list

Codeforces all useful blogs:
[I compiled a list of almost all useful blogs ever published on Codeforces [update: till 09.06.2021] - Codeforces](https://codeforces.com/blog/entry/91363)

Youtube channels: 
[Abdul Bari - YouTube](https://www.youtube.com/@abdul_bari)
[Algorithms Conquered - YouTube](https://www.youtube.com/@AlgorithmsConquered)
[CF Step - YouTube](https://www.youtube.com/@cfstepofficial)
[Errichto Algorithms - YouTube](https://www.youtube.com/@Errichto)
[Colin Galen - YouTube](https://www.youtube.com/@ColinGalen)
[Binary Box - YouTube](https://www.youtube.com/@binarybox3681)

Japanese ICPC contest website: 
[OI Wiki - OI Wiki (oi-wiki.org)](https://oi-wiki.org/)

[Kyopro Encyclopedia of Algorithms | Kyopro Encyclopedia of Algorithms (noshi91.github.io)](https://noshi91.github.io/algorithm-encyclopedia/)

[NeetCode](https://neetcode.io/)
[AlgoMap - Free Data Structures & Algorithms Roadmap](https://algomap.io/)


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
### Swap 2 numbers using bit manupilation
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



